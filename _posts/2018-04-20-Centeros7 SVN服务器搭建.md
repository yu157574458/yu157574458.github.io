---
layout: post
title: "CenterOS7 SVN服务器搭建"
description: "CenterOS7 SVN服务器搭建"
categories: [JAVA基础]
tags: [SVN,linux]
redirect_from:
  - /2018/04/20/
---
> 简介：很久之前在阿里云服务器上搭建过SVN服务器，这次买了腾讯云，重新搭建了SVN服务器，操作更熟练，也有了新的收获，特次记录下过程，和遇到的问题。  
> 同时推荐两个好用的软件：SecureCRT(用来操作liux命令)和FlashFXP（操作Linux系统文件）
> 

## SVN服务器：
svn的服务形式应该是有两种:

1. 通过svnserve建立的，访问方式 svn://ip:port 端口默认是3690
2. 通过httpd+svn，这种形式通过apache httpd或者其他的web服务器的扩展模块,进行svn管理。svn的账号是Apache http验证的，相当于svn服务器授权给了Apache http，只要Apache http能验证通过，就有权限访问。
3. 需要明确的一点是：两个是独立的认证方式 。svn仓库目录下的是未做任何处理的原始字符串，而后者却做了加密处理。

## 搭建过程
第一：yum安装svn  
运行yum install -y subversion即可。

第二：验证安装时候完成  
svnserve --version 

第三：创建svn版本库
mkdir /home/svn   
svnadmin create /home/svn/project  
我这里将svn作为所有版本库的目录，并创建了一个名为project的版本库 

第四：配置svnserve.conf（svn配置文件）
每个版本库创建之后都会生成svnserve.conf主要配置文件  
vim /home/svn/project/conf/svnserve.conf    
如： 
    anon-access = none    //控制非鉴权用户访问版本库的权限  
    auth-access = write   //控制鉴权用户访问版本库的权限  
    password-db = /home/svn/project/conf/passwd  //指定用户名口令文件名   
    authz-db = /home/svn/project/conf/authz   //指定权限配置文件名   
    realm =SVNSERVE QiYao Authorization SVN   //指定版本库的认证域，即在登录时提  
**要启用这些功能，需要将注释去除，注意最前面不要有空格**

第五：编辑passwd（账号密码文件）  
vim /home/svn/project/conf/passwd  
如：   
    [users]  
    # harry = harryssecret  
    # sally = sallyssecret  
    yuqichao = yuqichao  
    huyaowei = huyaowei  
    gaoji = gaoji  
    test = test  

第六：编辑authz（分组角色权限）  
vim /home/svn/project/conf/authz   
如：   
    [groups]
    admin = yuqichao,huyaowei,gaoji  
    user = test   
    [project:/]  
    @admin = rw   
    @user = r   
    
第七：启动和停止SVN服务
启动SVN服务：   
svnserve -dr /home/svn    
查看SVN服务：   
ps aux|grep svnserve   
停止SVN服务：   
killall svnserve

第八：访问路径  
SVN客户端可直接通过后面地址访问：svn://你的服务器ip/project   

## 搭建SVN ＋ Apache 服务器
一般公司访问都是通过http访问，可能是更保密点。  
上面如果都顺利搭建成功，后面需要加几步，即可实现http访问

1  安装httpd  
安装httpd服务：   
yum install httpd   
检查httpd是否安装成功：   
httpd -version

2  安装mod_dav_svn    
mod_dav_svn是apache服务器访问svn的一个模块。通过yum安装：    
yum install mod_dav_svn   
安装成功后，会有mod_dav_svn.so和mod_authz_svn.so两个文件。   
>  # find / -name mod_dav_svn.so   
> /usr/lib64/httpd/modules/mod_dav_svn.so   
>         
>  # find / -name mod_authz_svn.so   
>  /usr/lib64/httpd/modules/mod_authz_svn.so   

3  授权   
 修改svn仓库的用户组为apache：    
 chown -R apache:apache /home/svn/project/   
 创建用户文件passwd：    
  
    touch /home/svn/passwd  #创建用户文件   
    htpasswd /home/svn/passwd admin  #创建用户admin   
    htpasswd /home/svn/passwd guest  #创建用户guest   
    
    cat /home/svn/passwd 
    admin:$apr1$UCkPzZ2x$tnDk2rgZoiaURPzO2e57t0
    guest:$apr1$vX1RIUq6$OKS1bqKZSptzsPDYUOJ5x.   
 
 创建权限文件authz：   

    cp /home/svn/spring-hello-world/conf/authz /var/www/svn/authz
    
    cat /home/svn/authz 
    [/]
    admin = rw
    guest = r

4  配置httpd

touch /etc/httpd/conf.d/subversion.conf   

cat /etc/httpd/conf.d/subversion.conf  
 
    <Location /svn>
    DAV svn
    SVNParentPath /home/svn

    AuthType Basic
    AuthName "Authorization SVN"
    AuthzSVNAccessFile /home/authz
    AuthUserFile /home/svn/passwd
    Require valid-user
    </Location>

5  启动httpd服务   
systemctl start httpd.service

6  访问路径   
http://你的服务器ip/svn/project

## SVN服务器常用命令
//启动SVN         svnserve -d -r /home/svn/   
//查看启动端口    netstat -antp |grep svn   或    ps aux|grep svnserve   
//查看SVN进程    # ps -ef|grep svn|grep -v grep   
//检测SVN 端口   # netstat -ln |grep 3690   
//停止所有SVN    # killall svnserve    
//查看svn进程id来关闭SVN  # ps -aux|grep svn     #kill  id（第一列）   


// 启动httpd服务     systemctl start httpd.service   
// 重启httpd服务     systemctl restart httpd.service     

## 注意事项
- 修改配置文件时，前面不要留空格   
- 阿里云需要在安全组添加端口   
- 返回403错误，可能是防火墙问题。增加防火墙规则  
    firewall-cmd --permanent --add-service=http   
    firewall-cmd --permanent --add-service=https   
    firewall-cmd --reload  
