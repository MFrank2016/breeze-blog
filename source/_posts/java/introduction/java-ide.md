---
title: 【Java入门篇】四、Java开发环境搭建——IDE
tags:
  - Java入门
  - Java
  - 环境搭建
categories:
  - 编程
toc: true
date: 2020-05-01 22:33:05
---

> 不相信自己的人 连努力的价值都没有。 --《火影忍者》

## 前言

前面我们已经安装好了 `JDK` ，现在 `Java` 这款大型开放式沙盒游戏已经安装在我们的电脑里了，接下来我们就要准备学习如何玩这个游戏了。

在入门阶段，我建议选择简单模式来进行，那么一个好的 `IDE` 是必不可少的。

## 什么是IDE

`IDE（integrated development enviroment）` 就是集成开发环境，是用于提供程序开发环境的应用程序，一般包括`代码编辑器`、`编译器`、`调试器`和`图形用户界面`等工具。集成了代码编写功能、分析功能、编译功能、调试功能等一体化的开发软件服务套。所有具备这一特性的软件或者软件套（组）都可以叫集成开发环境。

简单来说， `IDE` 就是将一系列工具集成到了一个应用里，让你的开发之旅变得更加容易。

## 为什么需要IDE

很多老玩家在指导刚入坑的新手时，都喜欢循循善诱，教导新手使用 `文本编辑器` 而非 `IDE` ，这样可以提高对整个编译流程的理解。

讲道理，确实是这样的，但是却将开发的复杂度增加了许多，对于新手而言，每增加一个步骤，就是增加了无数种失败的姿势，很多人的学习热情就消耗在了这种无关痛痒的小问题上，很容易产生挫败感，觉得这个游戏怎么这么难。

所以我个人认为，`开局一条狗，砍到99`的打法并不适合每一个人，对于新手而言，先给一个`新手套装`，再来做任务会更加轻松。

## 安装IDE

接下来，我们去官网下载 `IDEA` ：`http://www.jetbrains.com/idea/`

![java-ide-1.png](https://i.loli.net/2020/05/02/P2ZVXphwkvzrTqj.png)

![java-ide-2.png](https://i.loli.net/2020/05/02/amnrtAJkhV87yqb.png)

根据自己的系统进行选择安装即可，这里就不分系统进行介绍了，下载的时候，可以选 `ultimate` 版，也可以选 `community` 版，建议选择 `ultimate` 版。

下载好以后安装，要激活码的时候可以看一看这个地址：`http://idea.lanyus.com/` 使用前请将“0.0.0.0 account.jetbrains.com”添加到 `hosts` 文件中，然后输入激活码就能成功激活了。当然，此方法仅供学习研究使用，有条件的盆友还是自觉购买正版产品吧。如果该方法已失效，则需要自行搜索[其它姿势](https://www.baidu.com/s?wd=idea%20%E6%BF%80%E6%B4%BB%E7%A0%81)了。

安装的时候，所有的都按默认选项进行安装即可。

## 新建项目

然后开始我们的第一个新手任务——`HelloWorld`。

![java-ide-3.png](https://i.loli.net/2020/05/02/IvMY16wh5pU9WNE.png)

![java-ide-4.png](https://i.loli.net/2020/05/02/CrWpUA13NszeytF.png)

![java-ide-5.png](https://i.loli.net/2020/05/02/qhDGHiVAOawZk9J.png)

创建好以后，右键src文件夹，添加package，名字叫hello，然后在package里添加HelloWorld类

![java-ide-6.png](https://i.loli.net/2020/05/02/MKGcT3CD7t2msRH.png)

![java-ide-7.png](https://i.loli.net/2020/05/02/buyAFqTGB9QjEzU.png)

![java-ide-8.png](https://i.loli.net/2020/05/02/nGU3f2zLcNHlXoT.png)

然后在文件里放上代码：

```java
package hello;
import java.lang.System;

public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```

点击 `Run` ，运行程序，选择 `HelloWorld` ，代码就跑起来了。

![java-ide-9.png](https://i.loli.net/2020/05/02/O8f6ePSNw4BWFph.png)

至此，`IDE` 设置完成，我们的第一个项目也完工。

![微信公众号](https://i.loli.net/2020/05/02/AfHOY5RXge9tlVo.png)