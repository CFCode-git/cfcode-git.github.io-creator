---
title: "Html入门笔记1"
date: 2019-10-31T18:16:17+08:00
draft: false
---

## 什么是HTML?它是谁发明的?
答：HTML是超文本标记语言（Hypertext Markup Language）,在1990年由李爵士（Tim Berners-Lee）发明。

## HTML起手式
起手式唯一招式："!+tab"

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
注：

1. !要用英文输入法。
2. `lang="en"`,language为英文，建议改成"zh-CN"。
3. `charset="UTF-8"`，支持各国字符的编码，不要改！
4. `<meta name="viewport" content="width=device-width, initial-scale=1.0">`防止页面缩放，适应手机屏幕。
5. `<meta http-equiv="X-UA-Compatible" content="ie=edge">`告诉浏览器采用最新的ie内核解析本页面。

## HTML章节标签

HTML章节标签包括：

* h1 ~ h6,<em>表示标题1 ~ 标题6</em>
* section,<em>表示章节</em>
* article,<em>表示文章</em>
* p,<em>表示段落</em>
* header,<em>表示页面的页眉（一般是页面的标题）</em>
* main,<em>表示页面的主要内容（页面主体部分）</em>
* footer,<em>表示页面的页脚（一般是版权声明、作者等）</em>
* aside,<em>表示旁支内容（与页面主体无关的部分,比如导航 条等）</em>
* div,<em>用于划分内容块</em>


## HTML全局属性

全局属性是所有标签都有的属性。

HTML全局属性有：

1. class,对标签分类,方便css对标签统一设置样式。
2. contenteditable,允许用户编辑。
3. hidden,隐藏标签内容。
4. id,类似class。
5. style,为标签元素设置样式。
6. tabindex,指定tab键选中的顺序。
7. title,当鼠标停留在元素上时,显示title的内容。

注：

* <b>class</b>

```
当一个元素有多个class属性时：（比如class = "middle word"）
[class = "middle"] {} --内容不一致就不匹配

.middle {} --只要是middle就匹配
```

* <b>hidden</b>

用css可以恢复显示 display：block；

* <b>id</b>

唯一性不严格，当存在两个相同的id时，html不会报错，css可以同时设置它们的样式，但js无法选中这两个相同的id。

* <b>style</b>

当js css style属性同时存在时，以js设置的样式为准，因为js可以覆盖html。

* <b>tabindex</b>

0表示最后选中，-1表示不要被选中。

* <b>title</b>

用于溢出省略的内容展现：

```
white-space:nowrap; --不要换行
text-overflow:ellipsis; --溢出的时候用省略号
overflow:hidden; --隐藏溢出的内容
```

## HTML内容标签

HTML内容标签包括：

1. ol+li --有序列表，ol中只能有li标签，不能有其他内容
2. ul+li --无序列表，ul中只能有li标签，不能有其他内容
3. dl+dt+dd --定义性列表，dt表示某个术语，dd是该术语的性质，dl是这些属于的集合。
4. pre --html文本中有多少个空格，输出是就保留多少个空格。
5. hr --分割线
6. br --换行
7. a --超链接
8. em --表示语气上的强调
9. strong --表示内容的重要性
10. code --展示源代码，通常与pre标签连用
11. quote --内联式的引用
12. blockquote --块级的引用

---
一些有用的网站：

[网道HTML教程](https://wangdoc.com/html/index.html)

[jirengu课程](https://xiedaimala.com/tasks/b6c18d7b-d7b4-463f-960b-40d732127133)
