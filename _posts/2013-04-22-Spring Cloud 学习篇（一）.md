---
layout: post
title: "Spring Cloud 学习篇（一）"
description: "Spring Cloud 学习篇（一）"
categories: [Spring]
tags: [spring cloud]
redirect_from:
  - /2018/03/18/
---
>简介：Spring Cloud是一套微服务开发和治理框架。Spring Cloud基于Spring Boot，为微服务体系开发中的架构问题，提供了一整套的解决方案——服务注册与发现，服务消费，服务保护与熔断，网关，分布式调用追踪，分布式配置管理等。
>
>springboot是为了更方便的让大家使用spring projects。 
目的是为了减少开发人员在项目配置上花费的时间，把时间更集中在业务层面。
Spring Boot的哲学就是约定大于配置。既然很多东西都是一样的，为什么还要去配置。

Spring boot 和Spring Cloud
spring boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring boot来实现。Spring boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring boot，属于依赖的关系。


Spring Cloud 组件运行：

![compare2](..\assets\images\logimages\20180318\springcloudcomponentprocedure.png)

##服务注册与发现
1. 创建调用关系的微服务
2. 使用Eureka实现服务注册与发现
3. Codeing Time  添加依赖 加注解

## 客户端负载均衡ribbn

## 声明式的Http Client Feign

## 微服务容错
- 雪崩效应
- 实现容错方案: 1.设置超时 2.使用断路器
- Coding Time 

## API网关

## 统一配置中心

