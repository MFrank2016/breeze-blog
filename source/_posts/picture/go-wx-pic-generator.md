---
title: 【Go语言绘图】 微信公众号文章封面图生成
tags:
  - go
categories:
  - 编程
toc: true
top: true
cover: true
date: 2021-08-01 19:48:23
---

## 想法来源

每次写文章都得花点时间找图，有点点麻烦，（其实就是懒。。。）。而且翻了翻之前的文章配图，大概是这个样子。

![](1.png)

emmm，风格还挺统一的。但身为正义凛然的公众号博主，老是用这样图，会有些图文不符，也不符合我这正襟危坐的人物形象，而且，做为一名程序猿，能用代码解决的事情，自然要用代码来解决。

最近看到有些技术类公众号，都使用了统一的图片模版，比如这样：

![](2.png)

左边放上logo，右边放字，简约大气。于是，想着自己也整一个。

![](3.png)

## 图片分析

要如何生成这样一张图呢，我们来简单来分析一下。

最底层是一张纯色的底图，这个不难，之前已经介绍过如何绘制纯色底图了，具体颜色可以用工具吸色看看。

第二层是文字，在前面的文章中，已经介绍过如何绘制文字，在这里刚好能用上。

第三层是logo，还没有介绍过将一张图覆盖在另一张图上的操作，本文会对此进行讲解。

这里还需要注意的就是文字跟logo的区域，在绘制时，需要将文本框和图片框的位置计算好，这个可以使用 Sketch 工具来完成。

![](4.jpg)

![](5.jpg)

![](6.jpg)

这样就得到了Logo和文字位置的具体信息。

接下来就可以开始写代码了

## 代码编写

首先，我们先绘制一个纯色的背景。RGB色值为：(63, 64, 87)，底图的宽和高通过 Sketch 工具测出分别为 1050和442。

```go
import (
	"github.com/fogleman/gg"
	"testing"
)

func TestDrawWxPic(t *testing.T){
	weight := 1050
	height := 442
	dc := gg.NewContext(weight, height)
	dc.SetRGB255(63, 64, 87)
	dc.Clear()
	dc.SavePNG("out.png")
}
```

输出图片如下：

![](7.png)

然后来绘制第二层的文字：

```go
import (
	"github.com/fogleman/gg"
	"golang.org/x/image/font"
	"golang.org/x/image/font/opentype"
	"io/ioutil"
	"testing"
)

func TestDrawWxPic(t *testing.T){
	width := 1050
	height := 442
	dc := gg.NewContext(width, height)
	dc.SetRGB255(63, 64, 87)
	dc.Clear()

	fontFilePath := "/Users/bytedance/Downloads/FangZhengKaiTiJianTi.TTF"
	fontFace, err := getOpenTypeFontFace(fontFilePath, 76, 72)
	if err != nil {
		panic(err)
	}
	dc.SetFontFace(*fontFace)
	dc.SetRGB255(238, 241, 247)

	// 文本框最大宽度
	maxWordsWidth := 660.0
	x := 645.0
	y := 224.0
	dc.DrawStringWrapped("高并发的秘密武器 epoll 机制", x, y, 0.5, 0.6, maxWordsWidth, 1.1, gg.AlignCenter)
	dc.SavePNG("out.png")
}

func getOpenTypeFontFace(fontFilePath string, fontSize, dpi float64)(*font.Face, error){
	fontData, fontFileReadErr := ioutil.ReadFile(fontFilePath)
	if fontFileReadErr != nil {
		return nil, fontFileReadErr
	}
	otfFont, parseErr := opentype.Parse(fontData)
	if parseErr != nil {
		return nil, parseErr
	}
	otfFace, newFaceErr := opentype.NewFace(otfFont, &opentype.FaceOptions{
		Size: fontSize,
		DPI:  dpi,
	})
	if newFaceErr != nil {
		return nil, newFaceErr
	}
	return &otfFace, nil
}
```

**DrawStringWrapped** 方法已经在之前的文章中介绍过了，这里就不缀述了，文本框的位置可以通过 (x,y,ax,ay)这四个参数来调整。

输出效果如下：

![](8.png)

字体不太一样，位置是差不多了。

最后来把logo拼上去：

```go
import (
	"github.com/disintegration/imaging"
	"github.com/fogleman/gg"
	"golang.org/x/image/font"
	"golang.org/x/image/font/opentype"
	"io/ioutil"
	"testing"
)

func TestDrawWxPic(t *testing.T){
	...
	dc.DrawStringWrapped("高并发的秘密武器 epoll 机制", x, y, 0.5, 0.6, maxWordsWidth, 1.1, gg.AlignCenter)

	//开始压缩图片
	src, openErr := imaging.Open("logo.png")
	if openErr != nil {
		panic(openErr)
	}
	src = imaging.Resize(src, 240, 240, imaging.Lanczos)
	dc.DrawImageAnchored(src, 182, 220, 0.5, 0.5)
	dc.SavePNG("out.png")
}
```

`fogleman/gg` 包不支持对图片使用 resize 的操作，因此这里引入了一个新包，"github.com/disintegration/imaging”。效果如下：

![](9.png)

emm，看起来颜色有些不谐调，改一下背景色，然后放大一下logo试试：

![](10.png)

嗯，这样就好多了，把这段代码封装成方法，传入标题和文件保存位置，即可一键生成封面图。

完整代码如下：

```go
import (
	"github.com/disintegration/imaging"
	"github.com/fogleman/gg"
	"golang.org/x/image/font"
	"golang.org/x/image/font/opentype"
	"io/ioutil"
	"testing"
)

func TestDrawWxPic(t *testing.T){
	generateWxImage("微信公众号封面图\n一键生成", "result")
}

func generateWxImage(title string, savingFileName string) {
	width := 1050
	height := 442
	dc := gg.NewContext(width, height)
	dc.SetRGB255(47, 54, 66)
	dc.Clear()

	fontFilePath := "FangZhengKaiTiJianTi.TTF"
	fontFace, err := getOpenTypeFontFace(fontFilePath, 76, 72)
	if err != nil {
		panic(err)
	}
	dc.SetFontFace(*fontFace)
	dc.SetRGB255(238, 241, 247)

	// 文本框最大宽度
	maxWordsWidth := 660.0
	x := 665.0
	y := 224.0
	dc.DrawStringWrapped(title, x, y, 0.5, 0.6, maxWordsWidth, 1.1, gg.AlignCenter)

	//开始压缩图片
	src, openErr := imaging.Open("logo.png")
	if openErr != nil {
		panic(openErr)
	}
	src = imaging.Resize(src, 360, 360, imaging.Lanczos)

	dc.DrawImageAnchored(src, 182, 220, 0.5, 0.5)

	dc.SavePNG(savingFileName + ".png")
}

func getOpenTypeFontFace(fontFilePath string, fontSize, dpi float64)(*font.Face, error){
	fontData, fontFileReadErr := ioutil.ReadFile(fontFilePath)
	if fontFileReadErr != nil {
		return nil, fontFileReadErr
	}
	otfFont, parseErr := opentype.Parse(fontData)
	if parseErr != nil {
		return nil, parseErr
	}
	otfFace, newFaceErr := opentype.NewFace(otfFont, &opentype.FaceOptions{
		Size: fontSize,
		DPI:  dpi,
	})
	if newFaceErr != nil {
		return nil, newFaceErr
	}
	return &otfFace, nil
}
```

## 小结

回顾一下整个过程，其实只要想清楚最终想要的效果，然后把步骤拆解开来，代码其实写起来并不复杂，重要的还是逻辑，至于类似于如何实现图片裁剪、文字拼接、图片缩放，这些通常都有现成的库可以拿来就用。

当然，如果感兴趣也可以深入研究其中的实现原理。就像前面所说，了解这些工具，可以增加手中的牌，遇到问题时自然更加左右逢源。

最近的大部分时间都花在了健身上，很长时间都没有好好写博客了，接下来得好好写文章了，光有输入确实还不够，用输出反推输入，会更加有的放矢。

![](11.png)