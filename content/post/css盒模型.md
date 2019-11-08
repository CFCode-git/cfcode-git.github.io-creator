---
title: "CSS盒模型"
date: 2019-11-05T19:49:48+08:00
draft: false
---

# 什么是css盒模型？

![图1](/images/1.png)

根据[css 2.1 中文版](http://www.ayqy.net/doc/css2-1/box.html)文档的描述:  
CSS盒模型描述了一个为文档树中的元素生成的并根据视觉格式化模型进行布局的矩形框。

我们可以理解为css中每一个元素都是一个盒子，拥有四个区域，分别是：内容区(_content_)、内边距(_padding_)、边框(_border_)和外边距(_margin_)。
css盒模型**分两种：border-box 和 content-box。**  
他们的区别在于content-box的宽度只包含content内容区，border-box的宽度包含到border，它包括content内容区、padding内边距、border边框。

返回[CSS基础](../css基础)


