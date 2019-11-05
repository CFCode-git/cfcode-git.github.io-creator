---
title: "CSS基础"
date: 2019-11-05T20:03:57+08:00
draft: false
---

# CSS --层叠样式表
CSS(_Cascading Style Sheets_)是层叠样式表，所谓层叠有三层含义：  

1. 样式层叠：可多次对同一选择器进行声明。
2. 选择层叠：可用不同选择器对同一元素进行声明。
3. 文件层叠：可用多个文件层叠。

CSS2.1是IE支持的现在使用最广泛的版本，CSS3是现代版本，分模块进行升级，目前IE8只是部分支持。由于CSS版本众多，因此要确认我们所使用的CSS特性在某个浏览器是否支持，我们可以查询caniuse网站。[网页地址](https://caniuse.com/)

# CSS语法
CSS语法主要有两种：

~~~CSS
/*第一种*/
选择器 {
    属性名:属性值;
    /*注释*/
}
~~~
注：  
1. 要使用英文符号；
2. 要区分大小写；
3. 没有"//"注释；
4. 最后的";"虽然可以省略，但不要省略；
5. 浏览器会忽略错误，但不会报错。

~~~CSS
/*第二种*/
/*声明字符编码*/
@charset = "UTF-8"; 

/*导入CSS文件*/
@import url(2.css); 

/*媒体查询*/
@media(min-width:100px)and(max-width:200px){
    语法一
}  
~~~
注:  
1. charset必须放在第一行。
2. 前两个的@语法必须加";"。
3. charset是字符集的意思，但语句本身声明的是字符编码。

# CSS查资料
1. Google搜索:MDN + 技术名词；
2. Google搜索:CSS tricks + 技术名词；
3. Google搜索:张鑫旭 + 技术名词；
* **最权威资料：Google搜索:CSS spec**(_可以查看css2.1中文版_)

# CSS的调试
1. 使用W3C验证器。[在线验证器](https://jigsaw.w3.org/css-validator/)
2. 使用VScode/Webstorm看字体颜色。
3. 使用开发者工具看警告。(有黄色的警告标志证明肯定是语法错误；被划掉说明语法错误或者是样式被覆盖/忽略。)
4. _border调试法_

# border调试法

当怀疑某个元素出现问题：  

1. 给这个元素逐行加border。
2. border没出现：border前面的语法错误，或者选择器错误。
3. border出现，证明前面的选择器或语法没有问题。观察border是否符合预期。

# 如何练习CSS？
* PSD

Freepik中搜索PSD Web。[网页地址](https://www.freepik.com/search?query=web&type=psd)  

* 效果图
参考dribbble.com中的页面模仿。[网页地址](https://dribbble.com/)  

* 模仿常去的商业网站

# 文档流
首先，在新的CSS规范中不存在内联元素和块级元素的区别，所有元素都可以是内联元素或者块级元素，这取决于display的取值。

## 文档的流动方向
inline元素从左到右，到最右边才换行，**在最右边有可能折断跨两行；**  
block元素从上到下，每一个都独占一行；  
inline-block元素也是从左到右，**但不会跨两行。**

## 宽度
inline元素宽度为内部inline元素的和，不能用width指定宽度；  
block元素默认 _自动_ 计算宽度，可以用width指定；(要多宽有多宽)  
inline元素结合前两者的特点，可用width指定。(要多窄有多窄)
**注意的点：**block的默认宽度是auto，并不是100%，且任何时候都不要设置width为100%。

## 高度
inline元素的高度由line-height _间接_ 决定，跟height无关；  
block元素的高度由**内部文档流元素**决定，可以设置height；  
inline-block与block类似，可以设置height。

## 溢出
当内容区的高度或宽度大于元素(容器)设定的高度或宽度时，内容会溢出。  
处理手段：设置overflow属性  
auto --自动
scroll --永远显示滚动条
hidden --隐藏溢出的内容  
visible --显示溢出的部分  

overflow可以分为overflow-x和overflow-y。

## 脱离文档流
当设置css为如下属性时，元素将脱离文档流。

~~~CSS
float:left/right/center

position:absolute/fixed
~~~

# 盒模型
查看:  
[css盒模型](../css盒模型)

