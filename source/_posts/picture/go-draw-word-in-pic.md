---
title: 【Go语言绘图】图片添加文字（一）
tags:
  - go
categories:
  - 编程
toc: true
cover: true
date: 2020-12-20 22:45:35
---

前一篇讲解了利用gg包来进行图片旋转的操作，这一篇我们来看看怎么在图片上添加文字。

## 绘制纯色背景

首先，我们先绘制一个纯白色的背景，作为添加文字的背景板。

```go
package main

import "github.com/fogleman/gg"

func main() {
	const S = 1024
	dc := gg.NewContext(S, S)
	dc.SetRGB(0, 1, 1)
	dc.Clear()
	dc.SavePNG("out.png")
}
```

输出图片如下：

![](1.png)

这样我就得到了一张纯青色的背景图。回顾一下上一篇里绘制背景图的步骤：

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

我们是通过先绘制跟画布同样大小的矩形，然后将它的颜色进行填充来实现纯色背景效果的，但实际上使用 `Clear()` 方法便能直接使用当前颜色对画布进行填充。

查看一下 `Clear()` 方法便能发现，里面是通过调用 `draw.Draw()` 函数来实现的，这也是go语言自带的 `image` 包里很有用的一个函数，后面会有文章来做更详细的介绍。简单来说，`Clear()` 方法是通过调用`draw.Draw()` 函数，通过将纯色图片覆盖到原画布的方式来实现纯色背景的效果的。

```go
// Clear fills the entire image with the current color.
func (dc *Context) Clear() {
	src := image.NewUniform(dc.color)
	draw.Draw(dc.im, dc.im.Bounds(), src, image.ZP, draw.Src)
}
```

## 添加文字

背景板已经准备就绪，接下来，我们来添加一些文字。

```go
package main

import "github.com/fogleman/gg"

func main() {
	const S = 1024
	dc := gg.NewContext(S, S)
	dc.SetRGB(0, 1, 1)
	dc.Clear()
	dc.SetRGB(0, 0, 0)
	if err := dc.LoadFontFace("gilmer-heavy.ttf", 120); err != nil {
		panic(err)
	}
	dc.DrawString("Hello, world!", 0, S/2)
	dc.SavePNG("out.png")
}
```

输出如下，一个硕大、黑色的“Hello, World!”就出现在了图片中央。

![](2.png)

这里我们添加了三个步骤，首先是设置了字体颜色为黑色。

```go
dc.SetRGB(0, 0, 0)
```

然后加载了字体文件，这里需要注意的是，通过 `LoadFontFace()` 方法加载的字体文件只支持 `ttf` 后缀的文件，也就是 `true type font`，

```go
if err := dc.LoadFontFace("gilmer-heavy.ttf", 120); err != nil {
    panic(err)
}
```

里面的实现也比较简单：

```go
func (dc *Context) LoadFontFace(path string, points float64) error {
	face, err := LoadFontFace(path, points)
	if err == nil {
		dc.fontFace = face
		dc.fontHeight = points * 72 / 96
	}
	return err
}
```

内部调用了 `LoadFontFace()` 函数，在这个函数内部进行了字体文件读取，并用 `freetype` 包里的`Parse()`函数进行字体的加载，最后在调用 `NewFace()` 函数来创建一个 `font.Face` 对象，在外面的`LoadFontFace()`方法里，将这个对象保存在 `fontFace` 字段中，并且根据传入的`point`大小设置了一下字体高度。

至于为什么是乘以`72`然后除以`96`，这个查了一下资料，简单的说，字体的大小单位磅(`points`) 是`1/72`逻辑英寸，屏幕的分辨率是`96DPI`（96点每逻辑英寸），那么屏幕每个点就是`72/96`=`0.75`磅。

```go
func LoadFontFace(path string, points float64) (font.Face, error) {
	fontBytes, err := ioutil.ReadFile(path)
	if err != nil {
		return nil, err
	}
	f, err := truetype.Parse(fontBytes)
	if err != nil {
		return nil, err
	}
	face := truetype.NewFace(f, &truetype.Options{
		Size: points,
		// Hinting: font.HintingFull,
	})
	return face, nil
}
```

### 调整字体大小

如果想调整字体大小，也很简单，只需要调整`LoadFontFace()` 方法传入的值即可，让我们来调大一点字体看看效果。

```go
if err := dc.LoadFontFace("gilmer-heavy.ttf", 240); err != nil {
    panic(err)
}
```

![](3.png)

这样就大很多了。不知道聪明的你注意到了没有，在调用`dc.DrawString("Hello, world!", 0, S/2)`时，我们设置的坐标是 `(0, S/2)` ，也就是左侧边的正中心点，**这个位置刚好是绘制出来的文字的左下角的坐标**，这是需要注意的一点。

### 居中显示

如果想要文字居中显示怎么办呢？比如我们想要这个 `Hello,World!` 显示在图片的正中央，要怎么处理呢？一个笨办法当然是通过调整字体位置来实现这个效果，让我们先来试试：

```go
if err := dc.LoadFontFace("gilmer-heavy.ttf", 120); err != nil {
    panic(err)
}
dc.DrawString("Hello, world!", 130, S/2)
```

![](4.png)

通过多次调整，字体大小设置为`120`时，`x`的位置设置为`130`，基本上可以看起来是居中的。但这样的话每次换文字都得反复调整位置，显然不科学。

别慌，有一个方法可以得到文字的宽度，`MeasureString()` 可以得到在当前字体下指定字符串的宽度和高度，这个高度其实就是前面通过 `points * 72 / 96` 计算得到的，然后我们再将左下角的位置设置为`((S-sWidth)/2, (S+sHeight)/2)`即可实现文字居中的效果，注意y轴坐标是`(S+sHeight)/2`，因为文字的左上顶点位置y轴坐标应该是`(S-sHeight)/2`，左下顶点坐标只需要再加上字体高度即可得出。

```go
s := "Hello, world!"
sWidth, sHeight := dc.MeasureString(s)
dc.DrawString(s, (S-sWidth)/2, (S+sHeight)/2)
```

![](5.png)

这样看来，居中显示也不过如此嘛。但别高兴的太早，有没有想过，如果文字过长该怎么处理？比如我们来调整一下文字内容，再看下生成的效果。

```go
s := "Hello,world! Hello,ByteDancer!"
```

![](6.png)

文字已经超出边界了，显然不是理想的效果，这个时候有两种处理方法，一种是添加省略号，一种是换行。

### 单行长文本处理

先来说一下添加省略号的处理方案，听起来好像挺简单，但实际上处理起来也挺麻烦的。

首先需要确定一个文字展示的最大宽度，因为如果满打满算整行都塞满文字显然不好看。其次是要逐个字符进行宽度计算，并判断是否会超过最大宽度，最后截取并保留刚好小于最大宽度时的字符串（需要考虑省略号的宽度）。

我们来逐个处理。首先拍脑袋定一个文字最大宽度为图片宽度的`0.75`倍。

```go
maxTextWidth := S * 0.75
```

然后来逐个字符计算宽度，直到刚好大于最大宽度为止。

```go
func TruncateText(dc *gg.Context, originalText string, maxTextWidth float64) string {
	tmpStr := ""
	for i := 0; i < len(originalText); i++ {
		tmpStr = tmpStr + string(originalText[i])
		w, _ := dc.MeasureString(tmpStr)
		if w > maxTextWidth {
			return tmpStr[0 : i-1]
		}
	}
	return tmpStr
}
```

然后我们调整一下调用的地方。

```go
func main() {
	const S = 1024
	dc := gg.NewContext(S, S)
	dc.SetRGB(0, 1, 1)
	dc.Clear()
	dc.SetRGB(0, 0, 0)
	if err := dc.LoadFontFace("gilmer-heavy.ttf", 120); err != nil {
		panic(err)
	}
	s := "Hello,world! Hello,ByteDancer!"
	ellipsisWidth, _ := dc.MeasureString("...")
	maxTextWidth := S * 0.75
	s = TruncateText(dc, s, maxTextWidth - ellipsisWidth) + "..."
	fmt.Println(s)
	sWidth, sHeight := dc.MeasureString(s)

	dc.DrawString(s, (S-sWidth)/2, (S+sHeight)/2)
	dc.SavePNG("out.png")
}
```

这里我们先计算了省略号的宽度，然后用最大字符串宽度减去省略号宽度作为最大宽度传入，得到最终要展示的字符串。生成的效果如下：

![](7.png)

看起来好像没什么毛病，但如果我们把文字换成中文，情况可能就不一样了。我们换一个中文字体，然后把字符串设置成中文。

```go
if err := dc.LoadFontFace("方正楷体简体.ttf", 120); err != nil {
    panic(err)
}
s := "如果我们把文字换成中文"
```

就变成了这个样子。

![](8.png)

发现图片上只剩下了省略号，原因是中文字符串分割不正确导致出现了乱码，而这个乱码在字体里找不到对应的文字，所以无法展示。这时，需要先将字符串先转化为`rune`数组，或者通过直接对字符串使用 `for range` 遍历，可以避免在中文的情况出现乱码的情况。

```go
func TruncateText(dc *gg.Context, originalText string, maxTextWidth float64) string {
	tmpStr := ""
	result := make([]rune, 0)
	for _, r := range originalText {
		tmpStr = tmpStr + string(r)
		w, _ := dc.MeasureString(tmpStr)
		if w > maxTextWidth {
			if len(tmpStr) <= 1 {
				return ""
			} else {
				break
			}
		} else {
			result = append(result, r)
		}
	}
	return string(result)
}
```

这样文字就能按照我们的预期进行展示了。

![](9.png)

### 多行文本处理

接下来，我们来看看怎么处理多行文本，即当一行文字展示不下时，把文字切割成多行进行展示。如果我们仍旧使用之前的方法来处理的话，就需要先计算好每行展示的字以及行数，然后再进行展示。

```go
package main

import (
	"github.com/fogleman/gg"
	"strings"
)

func main() {
	const S = 1024
	dc := gg.NewContext(S, S)
	dc.SetRGB(0, 1, 1)
	dc.Clear()
	dc.SetRGB(0, 0, 0)
	if err := dc.LoadFontFace("/Users/bytedance/Downloads/方正楷体简体.ttf", 120); err != nil {
		panic(err)
	}
	s := "这是我的一个秘密，再简单不过的秘密：一个人只有用心去看，才能看到真实。事情的真相只用眼睛是看不见的。        --《小王子》"
	ellipsisWidth, _ := dc.MeasureString("...")

	maxTextWidth := S * 0.9
	lineSpace := 25.0
	maxLine := int(S / (dc.FontHeight() + lineSpace))

	line := 0
	lineTexts := make([]string, 0)
	for len(s) > 0 {
		line++
		if line > maxLine {
			break
		}
		if line == maxLine {
			sw, _ := dc.MeasureString(s)
			if sw > maxTextWidth {
				maxTextWidth -= ellipsisWidth
			}
		}
		lineText := TruncateText(dc, s, maxTextWidth)
		if line == maxLine && len(lineText) < len(s) {
			lineText += "..."
		}
		lineTexts = append(lineTexts, lineText)
		if len(lineText) >= len(s) {
			break
		}
		s = s[len(lineText):]
	}

	lineY := (S - dc.FontHeight()*float64(len(lineTexts)) - lineSpace*float64(len(lineTexts)-1)) / 2
	lineY += dc.FontHeight()
	for _, text := range lineTexts {
		sWidth, _ := dc.MeasureString(text)
		lineX := (S - sWidth) / 2
		dc.DrawString(text, lineX, lineY)
		lineY += dc.FontHeight() + lineSpace
	}

	dc.SavePNG("out.png")
}

func TruncateText(dc *gg.Context, originalText string, maxTextWidth float64) string {
	tmpStr := ""
	result := make([]rune, 0)
	for _, r := range originalText {
		tmpStr = tmpStr + string(r)
		w, _ := dc.MeasureString(tmpStr)
		if w > maxTextWidth {
			if len(tmpStr) <= 1 {
				return ""
			} else {
				break
			}
		} else {
			result = append(result, r)
		}
	}
	return string(result)
}
```

这段逻辑其实也很简单，首先根据行高和行间距计算出当前图片最多能展示多少行字，然后遍历需要展示的字符串进行逐行截取，截取出一行行的文字来。

遍历时有一个小细节，那就是判断是否已经到达最后一行，如果到达最后一行，则要考虑是否添加省略号了。

```go
//如果已经是最后一行，则需要判断剩余字符串是否仍旧超过最大宽度
if line == maxLine {
    sw, _ := dc.MeasureString(s)
    // 如果超过则需要在末尾添加省略号，截取的最大宽度需要减去省略号的宽度
    if sw > maxTextWidth {
        maxTextWidth -= ellipsisWidth
    }
}
lineText := TruncateText(dc, s, maxTextWidth)
// 如果是最后一行并且文字仍旧是被截取过，那么在末尾添加省略号
if line == maxLine && len(lineText) < len(s) {
    lineText += "..."
}
```

在绘制文本时，先考虑整个文本框的左上顶点位置，因为需要居中展示，每一行的宽度是变化的，X轴坐标是不确定的，但是Y轴坐标是可以先计算出来的，因为每一行的高度和行间距我们都已经知道了。整个文本框的高度就是`dc.FontHeight()*float64(len(lineTexts)) - lineSpace*float64(len(lineTexts)-1))` ，用图片高度减去文本框高度再除以2，就能得到左上顶点高度了。

```go
lineY := (S - dc.FontHeight()*float64(len(lineTexts)) - lineSpace*float64(len(lineTexts)-1)) / 2
```

然后开始逐行绘制文字，计算每一行的左下顶点X轴和Y轴坐标即可。

```go
lineY += dc.FontHeight()
for _, text := range lineTexts {
    sWidth, _ := dc.MeasureString(text)
    lineX := (S - sWidth) / 2
    dc.DrawString(text, lineX, lineY)
    lineY += dc.FontHeight() + lineSpace
}
```

最后的效果如下图：

![](10.png)

这样虽然实现了效果，但是显然有些太过复杂，我们还能再简化一下这个过程。

在gg库中，还有两个方法可以绘制文字，`DrawStringAnchored()` 和 `DrawStringWrapped()`。前者可以在指定一个点为偏移起点。后者则类似于一个文本框的效果，可以指定文本框中心点和文本框宽度，这些将在下一篇中进行介绍。

这里的处理没有考虑原文本中有换行符的情况，所以其实还不够完善，在处理时可以先对文本进行换行符分割，然后再依次进行上述处理。

## 小结

这一篇中，主要讲解了如何在纯色背景图上进行文字的绘制，说明了 `DrawString()` 方法和 `MeasureString()` 的使用，并利用它们来实现了文字居中的效果。在下一篇中，将对通过另外几个方法的讲解来了解文字绘制的更多技巧。

如果本篇内容对你有帮助，别忘了点赞关注加收藏～
