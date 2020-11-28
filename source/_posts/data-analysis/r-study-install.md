---
title: 【R语言入门】R语言环境搭建
tags:
  - R
categories:
  - 编程
  - 数据分析
toc: true
date: 2020-11-28 08:47:35
---

## 说明

`R` 语言是一个功能十分强大的工具，几乎绝大多数的数据分析工作都可以在 `R` 中完成，并且拥有很极强的绘图功能支持，能让你手中的数据以各种姿势进行可视化呈现，而且支持 `Windows`、`Mac OS`、`Linux` 系统，而且使用起来也比较简单方便。

如果想要开始学习数据分析，或者仅仅是想做出狂拽炫酷屌的数据分析图，那么 `R` 语言会是个不错的选择。

## R 下载与安装

打开 `https://cran.r-project.org/mirrors.html` ，根据自己所在的位置选择对应的镜像站，通常选择 `China` 下的镜像站。

![](2.png)

根据自己使用的平台，选择对应安装包进行下载安装即可。

如果是 `Windows` 选择 `base` 版本进行下载安装即可。安装过程全部选择默认选项即可。

![](3.png)
![](4.png)
![](5.png)

如果用的是 `Mac` ，则选择 `Download R for (Mac) OS X`，下载最新版本的安装包后进行默认安装即可。

![](6.png)

安装完成之后，你将会看到一个朴实无华的图标，没错，这就是 `R` 语言本尊了。

![](7.png)

## R studio 下载与安装

打开 `https://www.rstudio.com/products/rstudio/download/` ，选择 `Free` 版本进行下载。

![](8.png)

这里会根据你所在平台显示对应的下载链接，点击下载即可。

![](9.png)

安装时，除了安装位置，其余均选择默认选项即可。

安装好之后，你又能收获一个新图标，这次要更加圆润一点。

![](10.png)

## R 语言简单实例

主要工作已经完成，让我们动动小手，优雅的单击（或双击）`R Studio` 图标，来感受一下R 语言的魅力。

打开 `RStudio`，会在 `Consule` 面板看到 `R` 语言的版本、版权信息和一些有用的提示。

```
R version 4.0.3 (2020-10-10) -- "Bunny-Wunnies Freak Out"
Copyright (C) 2020 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin17.0 (64-bit)

R是自由软件，不带任何担保。
在某些条件下你可以将其自由散布。
用'license()'或'licence()'来看散布的详细条件。

R是个合作计划，有许多人为之做出了贡献.
用'contributors()'来看合作者的详细情况
用'citation()'会告诉你如何在出版物中正确地引用R或R程序包。

用'demo()'来看一些示范程序，用'help()'来阅读在线帮助文件，或
用'help.start()'通过HTML浏览器来看帮助文件。
用'q()'退出R.
```

整体界面如下图：

![](11.png)

在 `consule` 面板中输入：`example(plot)`，轻轻敲击几次回车，就能看到 `plot` 函数的一些实例了。

```R
> example(plot)

plot> Speed <- cars$speed

plot> Distance <- cars$dist

plot> plot(Speed, Distance, panel.first = grid(8, 8),
plot+      pch = 0, cex = 1.2, col = "blue")
按<Return>键来看下一个图: 

plot> plot(Speed, Distance,
plot+      panel.first = lines(stats::lowess(Speed, Distance), lty = "dashed"),
plot+      pch = 0, cex = 1.2, col = "blue")
按<Return>键来看下一个图: 

plot> ## Show the different plot types
plot> x <- 0:12

plot> y <- sin(pi/5 * x)

plot> op <- par(mfrow = c(3,3), mar = .1+ c(2,2,3,1))

plot> for (tp in c("p","l","b",  "c","o","h",  "s","S","n")) {
plot+    plot(y ~ x, type = tp, main = paste0("plot(*, type = \"", tp, "\")"))
plot+    if(tp == "S") {
plot+       lines(x, y, type = "s", col = "red", lty = 2)
plot+       mtext("lines(*, type = \"s\", ...)", col = "red", cex = 0.8)
plot+    }
plot+ }
按<Return>键来看下一个图: 

plot> par(op)

plot> ##--- Log-Log Plot  with  custom axes
plot> lx <- seq(1, 5, length = 41)

plot> yl <- expression(e^{-frac(1,2) * {log[10](x)}^2})

plot> y <- exp(-.5*lx^2)

plot> op <- par(mfrow = c(2,1), mar = par("mar")-c(1,0,2,0), mgp = c(2, .7, 0))

plot> plot(10^lx, y, log = "xy", type = "l", col = "purple",
plot+      main = "Log-Log plot", ylab = yl, xlab = "x")
按<Return>键来看下一个图: 

plot> plot(10^lx, y, log = "xy", type = "o", pch = ".", col = "forestgreen",
plot+      main = "Log-Log plot with custom axes", ylab = yl, xlab = "x",
plot+      axes = FALSE, frame.plot = TRUE)

plot> my.at <- 10^(1:5)

plot> axis(1, at = my.at, labels = formatC(my.at, format = "fg"))

plot> e.y <- -5:-1 ; at.y <- 10^e.y

plot> axis(2, at = at.y, col.axis = "red", las = 1,
plot+      labels = as.expression(lapply(e.y, function(E) bquote(10^.(E)))))

plot> par(op)
> 
```

下面是输出的图片：

![](12.png)
![](13.png)
![](14.png)
![](15.png)

这是基础绘图函数 `plot` 的几个示例，可以看出它能支持的图形已经有许多了，后面会有专门的文章来介绍 `plot` 函数的详细使用方法。

## 小结

到此为止，`R` 语言就已经顺利的收入囊中了，恭喜你，又掌握了一门语言（的 `Hello world`）了。【此处应有掌声】

接下来，会继续介绍 `R` 语言的基本用法和其中比较重要的函数使用方法，目标是能使用 `R` 语言对数据进行初步分析，以便能在生活和工作之中有所应用。

之所以开始写 `R` 语言相关的文章，是因为在工作中察觉到了数据的重要性，虽说应该让专业的人来做专业的事情，但如果对此一无所知，又怎么知道专业的人能够做什么事情呢，何况如果一点点小事情就要找数据的同学提需求未免不太合适，自己先有头绪和初步验证后也许会更有效率。而且技多不压身嘛。加之在大学时就对数据分析感兴趣，也曾经学过 `R` 语言，现在算是重温和复习吧。

