---
title: 【Java入门篇】三、Java开发环境搭建——Mac篇
tags:
  - Java入门
  - Java
  - 环境搭建
categories:
  - 编程
toc: true
date: 2020-05-01 22:33:04
---

> 人非圣贤，孰能无过？过而能改，善莫大焉。 --《左传》

## 步骤说明

主要分为以下几个步骤：

1.到 `Oracle` 官网下载 `JDK` 安装包。(我在这里选择的是 `JDK1.8`，也可以选择其它版本)

2.打开获取到的安装包按步骤安装到系统上。

3.配置系统的环境变量。

4.验证 `JDK` 是否安装成功。

【由于电脑上已经安装过了 `JDK1.8`，所以偷懒把别人的文章搬过来了，[原文链接](https://blog.csdn.net/deliciousion/article/details/78046007)】

## 一、下载JDK8

通过下面 `Oracle` 官网找到对应的 `JDK1.8` 安装包

https://www.oracle.com/index.html

打开后如下所示，`Oracle` 主页内容经常变动，读者打开后很有可能不一样。

![1-jdk-mac.png](https://i.loli.net/2020/05/02/munYUqTkJg5IxDR.png)

拉到页面底部，找到“Download Java for Developers”，如下红框所示。

![2-jdk-mac.png](https://i.loli.net/2020/05/02/GOvJdf7zLWrTCEb.png)

点开链接后，如下图所示，再点击红框位置，只下载 `JDK1.8` ，红框右边的链接是 `JDK1.8` 加上 `NetBeans` ，一个挺好用的 `JAVA IDE`（集成化开发环境），有需要的可以下载。

![3-jdk-mac.png](https://i.loli.net/2020/05/02/FakO2WjeMR1hi79.png)

进入页面后第一步，点击“Accept License Agreement”同意许可证协议。第二步选择 `JDK` 对应的操作系统。本次选择“MAC OS X”，最后把相应的安装包下载到本地。

![4-jdk-mac.png](https://i.loli.net/2020/05/02/I3XBT1vUJQGHDcK.png)

## 二、安装JDK

下载完成后，我们得到一个dmg的安装包，如下图所示，名称为 `jdk-8u144-macosx-x64.dmg` ，表示这是 `Java 8` 版本号为 `144` 的 `JDK`安装包，如果选择的是其它版本，名称也会有所不同。

![5-jdk-mac.png](https://i.loli.net/2020/05/02/PSbdcZGVfY1n4Oz.png)

双击 `dmg` 安装包，打开如下图所示窗口。按照红框的提示，便可轻松完成安装。

![6-jdk-mac.png](https://i.loli.net/2020/05/02/bDiVGkgKOrQJS5w.png)

再双击中间的 `pkg` 文件，开始安装，如下图所示。

![7-jdk-mac.png](https://i.loli.net/2020/05/02/kMa9bSmKOY2seFD.png)

![9-jdk-mac.png](https://i.loli.net/2020/05/02/ICbJmudG73f52lx.png)

![10-jdk-mac.png](https://i.loli.net/2020/05/02/e92Dx1XPzBI5WVO.png)

## 三、配置系统的环境变量

上一步骤，实标上，我们只是把 `JDK1.8` 的文件复制到操作系统上。但是我们如果要在 `terminal` 终端上使用 `JAVA` 命令，还需要让其它应用知道 `JDK1.8` 环境的存在，那我们还需要配置系统的环境变量。

首先我们得知道 `JDK` 目录安装在哪里，按照下面的路径我们可以找到 `JDK` 的主目录，如下图所示。这里有两个目录是因为本机较早前安装过早期版本的 `JDK1.8`。

```bash
/Library/Java/JavaVirtualMachines
```

![11-jdk-mac.png](https://i.loli.net/2020/05/02/ewjVCx6Wdiz7hE5.png)

由于 `MAC` 文件系统结构，与 `WINDOWS` 有所不一样，所以 `JDK`的真实主目录如下

```bash
/Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home
```

打开 `terminal` 终端，默认打开在自身 `home` 目录下，也可通过 `cd` 命令直接跳到主目录。

![12-jdk-mac.png](https://i.loli.net/2020/05/02/bFOD8ViUStJYdBW.png)

通过 `vim .bash_profile` 命令打开启动文件，修改内容

![13-jdk-mac.png](https://i.loli.net/2020/05/02/u51VTvULm6HtJcn.png)

进入 `vim`，按 `i` 键进入编辑状态。添加如下内容，如下图所示。再按 `ESC` 键，输入“:wq”保存退出。配置系统环境变量结束。

![14-jdk-mac.png](https://i.loli.net/2020/05/02/93vEFplMf4Vjg8O.png)

添加如下内容：

```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home
```

注意将 `jdk1.8.0_45` 这里改为你下载的 `JDK` 版本，不清楚的话，到 `/Library/Java/JavaVirtualMachines/` 这个目录下找找。

## 四、验证JDK1.8是否安装成功。

在终端输入 `java`,有如下画面，证明配置成功

![15-jdk-mac.png](https://i.loli.net/2020/05/02/S3X7JwZfONlvqsM.png)

或输入 `java -version`，会出现版本信息

![16-jdk-mac.png](https://i.loli.net/2020/05/02/zlKysIfgXDL1E4C.png)

至此，整个安装 `JDK` 过程结束。

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)