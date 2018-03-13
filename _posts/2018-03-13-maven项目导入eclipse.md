---
layout: post
title: "maven项目导入eclpse，及maven学习"
description: "maven项目导入,及maven学习"
categories: [环境搭建]
tags: [maven,eclipse,svn]
redirect_from:
  - /2018/03/13/
---
> 简介：开发过程中需要eclipse从SVN检出maven web项目

## 从svn导入需要注意点
从svn检出了项目之后并不是maven的基本格式，需要自己修改过才行
>* 第一步，从svn检出项目，作为新项目检出
>* 第二步，新项目要选Dynamic Web Project
>* 第三步，转成maven项目
>* 第四步，修改pom.xml,主要是保证JDK一致

## Maven知识

Maven是基于项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。
Maven是跨平台的项目管理工具。主要服务于基于Java平台的项目构建，依赖管理和项目信息管理。
Maven主要有两个功能：
1、项目构建
2、依赖管理
### 一、Maven安装与环境配置（Windows）
详细可见：[Maven安装与环境配置（Windows）](http://blog.csdn.net/xyang81/article/details/51487939 "Maven安装与环境配置（Windows）")

### 二、理解“仓库”

当Maven运行过程中的各种配置，例如pom.xml，不想绑定到一个固定的project或者要分配给用户时，我们使用settings.xml中的settings元素来确定这些配置。这包含了本地仓库位置，远程仓库服务器以及认证信息等。
settings.xml存在于两个地方：

1. 安装的地方：$M2_HOME/conf/settings.xml
2. 用户的目录：${user.home}/.m2/settings.xml

前者叫做全局配置，后者被称为用户配置。

如果两者都存在，它们的内容将被合并，并且**用户范围**的settings.xml优先。

目的就是让个人开发特定的项目能够区分不同版本的jar包。

如果你偶尔需要创建用户范围的settings，你可以简单的copy Maven安装路径下的settings到目录${user.home}/.m2。Maven默认的settings.xml是一个包含了注释和例子的模板，你可以快速的修改它来达到你的要求。

发现很多第三方的项目默认的setting配置都是用户目录/.m2/settings.xml

所以为了方便，需要自己创建.m2文件夹，并在其中配置settings.xml

网上的教程就是使用命令
 >   `mvn help:system`

使用之后，发现并没有生成.m2文件夹

查找很多之后发现，必须把默认的maven里面的本地存储设置为默认的，就是不要设置
>   ` <localRepository>D:\maven\repository</localRepository>`

这一行注释或取消掉，再执行mvn help:system命令就OK了。