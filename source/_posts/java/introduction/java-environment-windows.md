---
title: 【Java入门篇】二、Java开发环境搭建——Windows篇
tags:
  - Java入门
  - Java
  - 环境搭建
categories:
  - 编程
toc: true
date: 2020-05-01 22:33:03
---

> 你为了你的正义，我为了我的正义。 -- 《火影忍者》

## 一、安装JDK

官网下载链接：https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

![1-jdk-windows.png](https://i.loli.net/2020/05/02/CvASgTp3BQFGlks.png)

## 二、配置环境变量

需要配置以下几个环境变量：

|环境变量名|说明|
|-|-|
|JAVA_HOME|配置JDK安装路径|
|PATH|配置JDK命令文件的位置|
|CLASSPATH|配置类库文件的位置｜

### 1、我的电脑（右键）-->属性-->高级系统设置

![2-jdk-windows.png](https://i.loli.net/2020/05/02/KkW2HEcewYmfyLC.png)

### 2、环境变量-->新建

![3-jdk-windows.png](https://i.loli.net/2020/05/02/Iybmol7AfZ2JjPV.png)

![4-jdk-windows.png](https://i.loli.net/2020/05/02/iaUDGuIA2dFVLsb.png)

![5-jdk-windows.png](https://i.loli.net/2020/05/02/Fm6WAQtRjKyxPv3.png)

(1)新建->变量名"JAVA_HOME"，变量值"C:/Java/jdk1.8.0_144"（即JDK的安装路径），在此，我安装的是 `java8` 版本，如果选择其它版本，设置对应的路径和名称即可。 

(2)编辑->变量名"Path"，在原变量值的最后面加上“;%JAVA_HOME%/bin;%JAVA_HOME%/jre/bin” 

(3)新建->变量名“CLASSPATH”,变量值“.;%JAVA_HOME%/lib;%JAVA_HOME%/lib/dt.jar;%JAVA_HOME%/lib/tools.jar”

### 3、确认环境配置是否正确

在控制台分别输入 `java`，`javac`，`java -version` 命令，出现如下所示的 `JDK` 的编译器信息，包括修改命令的语法和参数选项等信息。

`java` 命令：

![6-jdk-windows.png](https://i.loli.net/2020/05/02/4rDZIYn6yL5lGEp.png)

`javac` 命令：

![7-jdk-windows.png](https://i.loli.net/2020/05/02/WagrjY8X9AcwPNI.png)

`java -version` 命令：

![8-jdk-windows.png](https://i.loli.net/2020/05/02/wBrWpu7P8KIA1c6.png)

### 4、在控制台下验证第一个java程序：

右键--》新建--》文本文档

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

用记事本编写好，点击“保存”，并保存在桌面后，先在控制台中进入桌面的目录。

```bash
C:
cd /Users/[用户名]/Desktop
```

上面的[用户名]改成你的计算机用户名即可，不清楚的话打开我的电脑，进C盘目录：`C:/Users` 找一下。

输入 `javac Test.java` 和 `java Test` 命令，即可运行程序（打印出结果“Hello Java”）。

![9-jdk-windows.png](https://i.loli.net/2020/05/02/InRJXZb54T1veuc.png)

至此，`Java` 开发环境搭建成功。

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)