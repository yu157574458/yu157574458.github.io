---
layout: post
title: "JVM、JRE和JDK三者间的区别和联系"
description: " 针对初学者了解JVM、JRE和JDK三者间的区别和联系"
categories: [java基础]
tags: [jdk,jre,jvm]
redirect_from:
  - /2013/04/24/
---
> 简介：我们利用JDK（调用JAVA API）开发了属于我们自己的JAVA程序后，通过JDK中的编译程序（javac）将我们的文本java文件编译成JAVA字节码，在JRE上运行这些JAVA字节码，JVM解析这些字节码，映射到CPU指令集或OS的系统调用。

## JDK(Java Development ToolKit)
> Java开发工具包，它除了包括JRE和JVM外，还包括java(用于执行.class文件)、javac(用于将.java文件编译成.class文件)等工具和JAVA基础的类库。这些工具能够很好地帮助我们进行Java开发。

## JRE(Java  Runtime  Enviromental)
> Java运行时环境，它包括了JVM和Java的一些常用的类库，但不包含开发工具(JDK)--编译器、调试器和其它工具。
它包括了JVM和Java的一些常用的类库，JVM就是上面所说的Java虚拟机，而类库就是我们在编写好Java程序后所依赖的核心类和支持文件，没有这些类库，我们编写好Java程序就没法正常执行，可以说JRE是运行Java程序的最小环境。 
 
## JVM(Java Virtual Mechinal)
> JVM是JRE的一部分，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。JVM有自己完善的硬件架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。JVM 的主要工作是解释自己的指令集（即字节码）并映射到本地的 CPU 的指令集或 OS 的系统调用。Java语言是跨平台运行的，其实就是不同的操作系统，使用不同的JVM映射规则，让其与操作系统无关，完成了跨平台性。JVM 对上层的 Java 源文件是不关心的，它关注的只是由源文件生成的类文件（ class file）。类文件的组成包括 JVM 指令集，符号表以及一些补助信息。


## jdk和jre关系
如果你需要运行java程序，只需安装JRE就可以了。如果你需要编写java程序，需要安装JDK。

至于jvm，jdk和jre都包含jvm。简单来说jvm就是一个软件。一个什么软件呢？一个可以运行Java的软件。我们在将.java编译后，会生成相应的.class文件，那么，问题来了，什么问题呢？就是这个.class文件怎么运行？运行在哪里？答案就是JVM。JVM就是加载并运行.class文件的软件。