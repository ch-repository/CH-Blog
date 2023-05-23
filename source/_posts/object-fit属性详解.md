---
title: object-fit属性详解
date: 2023-05-23 09:05:51
categories: CSS
tags: CSS
comments: false
---

我们并不总是能够为HTML元素加载不同大小的图像，如果我们使用与图像纵横比不成比例的宽度和导读，则图像可能会被压缩或者拉伸。为解决此问题，我们可以为`img`元素添加`object-fit`属性。

当图像的纵横比与包含元素的宽度和高度不一致时，我们并不总是需要添加不同大小的图像。我们可以通过剪裁图片的方法，保留图像的纵横比来防止图片被挤压，而CSS的`object-fit`就具有这样的作用。

### CSS object-fit
该`object-fit`属性定义了被替换元素的内容（例如`img`或`video`标签应该如何调整大小以适应其容器）。`object-fit`的默认值是`fill`，这可能会导致图像被挤压或拉伸。

`object-fit`的几个属性值：

1. [contain](#contain)
2. [cover](#cover)
3. [fill](#fill)
4. [none](#none)
5. [cover](#cover)
6. [cover](#cover)

#### <a id="contain">contain</a>

使用`object-fit: contain`，图像将调整大小以适应其容器的纵横比。如果图像的纵横比与容器的不匹配，那么它将被加黑。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523144011.png" />



#### <a id="cover">cover</a>

使用`object-fit: cover`，图像也将被调整大小以适应其容器的纵横比，如果图像的纵横比与容器的不匹配，那么它就会被裁剪以适应容器。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523144222.png" />



#### <a id="fill">fill</a>

使用`object-fit: fill`，图像将被调整大小以适应其容器的纵横比，如果图像的纵横比与容器的不匹配，它将被挤压或者拉伸。`object-fit`的默认值。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523144412.png" />



#### <a id="none">none</a>

使用`object-fit: none`时，图像根本不会被调整大小，即不会拉伸也不会被压缩。它像`cover`一样工作，但它不尊重其容器的纵横比。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523144632.png" />



> 额外属性：除了`object-fit`，我们还有`object-position`属性，它负责将图片定位在容器中。

### object-position

`object-position`属性的工作方式类似于CSS的`background-position`属性：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523144910.png" />

当容器的纵横比垂直较大时，`top`和`bottom`关键字也有效：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/20230523145015.png" />



参考文章：http://www.webkaka.com/tutorial/html/2022/0321237/