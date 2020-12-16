---
title: 【Go语言绘图】图片的旋转
tags:
  - go
categories:
  - 编程
toc: true
top: true
cover: true
date: 2020-12-16 22:50:00
---

在上一篇中，我们了解了gg库的基本使用，包括调整大小、调整圆形参数、设置颜色、保存图片、加载图片和裁剪。这一篇我们来学习一下图片的旋转。

## 加载图片

首先，我们先来一张黄图。

```go
func TestRotateImage(t *testing.T) {
	width := 1000
	height := 1000

	dc := gg.NewContext(width, height)
	dc.DrawRectangle(0, 0, float64(width), float64(width))
	dc.SetRGB255(255, 255, 0)
	dc.Fill()
	dc.SavePNG("test.png")
}
```

![](1.png)

然后加载好我们要旋转的图片，用的仍旧是我们上一篇中使用的图。

![](2.png)

```go
func TestRotateImage(t *testing.T) {
	im, err := gg.LoadImage("/Users/bytedance/Desktop/test.png")
	if err != nil {
		panic(err)
	}
	w := im.Bounds().Size().X
	h := im.Bounds().Size().Y

	width := 2 * w
	height := 2 * h

	dc := gg.NewContext(width, height)
	dc.DrawRectangle(0, 0, float64(width), float64(width))
	dc.SetRGB255(255, 255, 0)
	dc.Fill()

	dc.DrawImage(im, width/4, height/4)
	dc.SavePNG("test.png")
}
```

这里为了更好的看到旋转的效果，对之前的代码做了一些调整。把画布大小设置为2倍图片的长宽。

```go
width := 2 * w
height := 2 * h

dc := gg.NewContext(width, height)
```

然后绘制了一个矩形，并且将它的颜色填充为黄色（因为图片比较白，用黑色背景更容易看到边界）。

```go
dc.DrawRectangle(0, 0, float64(2*h), float64(2*w))
dc.SetRGB255(255, 255, 0)
dc.Fill()
```

顺便纠正一下上一篇中的遗漏的点，使用 `setRGB()` 方法来设置颜色确实需要使用转换函数来将RGB值进行映射，但还有另一个方法 `SetRGB255()` 可以直接设置RGB值，就不需要先进行一次转换了。

然后我们将图片加载到了正中心的位置，`(w/4,h/4)` 对应图片左上角在画布上的位置。

```go
dc.DrawImage(im, width/4, height/4)
```

输出的图片如下：

![](3.png)

## 旋转图片

图片加载好了，下面我们开始添加一个旋转操作。

```go
func TestRotateImage(t *testing.T) {
	im, err := gg.LoadImage("/Users/bytedance/Desktop/test.png")
	if err != nil {
		panic(err)
	}
	w := im.Bounds().Size().X
	h := im.Bounds().Size().Y

	width := 2 * w
	height := 2 * h

	dc := gg.NewContext(width, height)
	dc.DrawRectangle(0, 0, float64(width), float64(width))
	dc.SetRGB255(255, 255, 0)
	dc.Fill()

	dc.Rotate(45)
	dc.DrawImage(im, width/4, height/4)
	dc.SavePNG("test.png")
}
```

其实只添加了一行代码，就是在加载图片前先调用了 `Rotate()` 方法。想象之中，我们会把图片旋转45度，但实际上是这样的：

![](4.png)

好像不太符合预期，实际上，仔细研究一下就会发现，这里的旋转是围绕原点也就是整个画布的左上角进行旋转的，那我想要它围绕中心点旋转该怎么办呢？别慌，换一个方法就可以了。`RotateAbout()` 方法可以指定图片的旋转中心点，换这个来试试看：

```go
func TestRotateImage(t *testing.T) {
	im, err := gg.LoadImage("/Users/bytedance/Desktop/test.png")
	if err != nil {
		panic(err)
	}
	w := im.Bounds().Size().X
	h := im.Bounds().Size().Y

	width := 2 * w
	height := 2 * h

	dc := gg.NewContext(width, height)
	dc.DrawRectangle(0, 0, float64(width), float64(width))
	dc.SetRGB255(255, 255, 0)
	dc.Fill()

	dc.RotateAbout(45, float64(width/2), float64(height/2))
	dc.DrawImage(im, width/4, height/4)
	dc.SavePNG("test.png")
}
```

![](5.png)

这下图片确实绕中心点旋转了，但转45度好像不应该是这样的，再来看看这个方法的说明：

```
// RotateAbout updates the current matrix with a clockwise rotation.
// Rotation occurs about the specified point. Angle is specified in radians.
```

可以看到，第一个参数的意思其实代表的是弧度，而不是角度，所以想要旋转45度当然不能这么传，我们换一个姿势再试试。

```go
func TestRotateImage(t *testing.T) {
	im, err := gg.LoadImage("/Users/bytedance/Desktop/test.png")
	if err != nil {
		panic(err)
	}
	w := im.Bounds().Size().X
	h := im.Bounds().Size().Y

	width := 2 * w
	height := 2 * h

	dc := gg.NewContext(width, height)
	dc.DrawRectangle(0, 0, float64(width), float64(width))
	dc.SetRGB255(255, 255, 0)
	dc.Fill()

	dc.RotateAbout(gg.Radians(45), float64(width/2), float64(height/2))
	dc.DrawImage(im, width/4, height/4)
	dc.SavePNG("test.png")
}
```

![](6.png)

这下终于得到了我们想要的图。

## 总结

图片旋转其实很简单，只需要在绘制前调用 `Rotate()` 或 `RotateAbout()` 方法即可。但需要注意几点：

1. 旋转是顺时针旋转
2. `Rotate` 方法是绕左上角旋转
3. 第一个参数都代表的是弧度而不是角度

这样旋转我们也能掌握了，图片处理功能又进了一步。喜欢本文的朋友欢迎点赞收藏加关注～

![](7.png)
