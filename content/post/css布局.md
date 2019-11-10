---
title: "css布局"
date: 2019-11-08T06:36:14+08:00
draft: false
---

float布局、flex布局、grid布局。

# 布局分类
固定宽度布局：主要有960/1000/1024px;  
不固定宽度布局：靠文档流原理布局。(主要用于手机页面);  
响应式布局(混合布局)：既适应pc上的固定布局，也适应手机上的不固定布局。

# 布局思路

![css布局思路图](/images/css布局思路.png)

# float布局

**步骤**  
1. 给子元素加上`float:left | right;`  
2. 给父元素加上`.clearfix`  

**经验**  
1. 一般会预留一些空间或者最后一个元素width不写死。  
2. 不需要做响应式，因为手机页面没有IE，而float布局是专门为IE设计的。  
3. IE6/7存在双倍margin的bug，解决办法是  
   * 针对IE6/7对margin减半  
   * 再加一个`display:inline-block;`  
4. 用"outline"代替"border",可以使得边框不占据内部的空间(outline不在border-box的width计算范围内)  

~~~css
.float {
    float:left;
    margin-left:10px; /*一般浏览器会执行这一句,忽略下一句。*/
    _margin-left:5px; /*IE6/7认得这一句，因此会先执行上一句，然后执行这一句重设margin，注意下划线_*/
}
~~~

**代码示例**
~~~html
<!--html-->
<header class = "clearfix">
    <div class= "float">balabala</div>
</header>
~~~

~~~css
/*css*/
.clearfix:after {  /*":"或"::"都行，但IE8及以下只认得":"*/
    content:"";
    display:block;
    clear:both;
}
~~~

~~~css
/*小技巧：如果发现图片下面有多余的东西，可以用这句话让图片居中。*/
vertical-align:top |middle;

/*让块级元素居中*/
margin:0 auto;
/*下面这句更好，因为没有覆盖元素本身的上下外边距*/
margin-left:auto;
margin-right:auto;
~~~

# flex布局

container--容器(父元素) item--项目(容器内的子块；子元素)

**class = "container" 样式**

~~~css
/*设置flex布局*/
.container {
    display:flex | inline-flex;
}

/*item流动方向-主轴*/
.container {
    flex-direction:row | row-reverse | column | column-reverse;
}

/*折行，默认nowrap，如果不设置，所有元素都会挤在同一行。*/
.container {
    flex-wrap:nowrap | wrap | wrap-reverse;
}

/*主轴对齐方式 缩写：jc*/
.container {
    justify-content:flex-start | flex-end | center | space-between | space-around | space-evenly;
}

/*次轴对齐方式 缩写：ai*/
.container {
    align-items:flex-start | flex-end | center | stretch | baseline;
}

/*多行内容(比较少用)*/
.container {
    align-content:flex-start | flex-end | center | stretch | space-between | space-around;
}
~~~

**class = "item"**  
1. 用order值排序，由小到大排列；  
2. 用flex-grow控制"长胖"，设置的值越大，得到的多余空间越多。  
3. 用flex-shrink控制"变瘦"，设置的值越大，在空间不够时压缩得越大。(默认是1，一般设置为0防止压缩)  
4. flex-basis控制宽度  
5. 缩写flex-grow flex-shrink flex-basis 为 flex。  
6. 通过align-self定制某个item的align-items。  
 
**经验**  
1. 永远不要把width和height写死。  
2. 用min/max-height和min/max-width。   
3. flex可以基本满足所有需求  
4. flex与margin-xxx:auto配合有意外效果。(往反方向写，比如向左靠就写margin-right:auto;)  

# 负margin
采用float和flex写平均布局时常常要用到负margin。  

![我的理解负margin](/images/-margin.jpg)

# grid布局

目前grid布局还没有普及。

**class = "container"**
~~~css
/*设置grid布局*/
.container {
    display:grid | inline-grid;
}

/*设置行&列*/
.container {
    grid-template-columns: 40px 50px auto 40px 50px; /*五列*/
    grid-template-rows:25% 100px auto; /*三行*/
    
    /*可以缩写*/
    grid-template-columns:240px repeat(4,120px); 
    /*相当于240px 120px 120px 120px 120px*/
}

/*可以给线取名但一般不这么做*/
.container {
    grid-template-columns: [first]40px [line2]50px [line3]auto [col4-start]40px [five]50px [end]; /*五列*/
    grid-template-rows:[row1-start]25% [row1-end]100px [third-line]auto [last-line]; /*三行*/
}
~~~

**class = "item"**

~~~css
/*设置范围*/
.item {
    grid-column-start:2;
    grid-column-end:five; /*从第二根线开始到名为【five】的线结束*/
    grid-row-start:row1-start;
    grid-row-end:3; /*从名为【row1-start】的线开始到第三根线结束*/
}
~~~

## fr --free space 任意空间
~~~css
.container {
    grid-template-columns:1fr 1fr 1fr;
}

/*也可以固定其中的某个宽度*/
.container {
    grid-template-columns:1fr 50px 1fr 1fr;
}
~~~

## grid-gap 设定中间的空隙

可以分别设定横向空隙`grid-row-gap`和纵向空隙`gird-column-gap`.

## 超级好用的属性 --分区：grid:grid-template-areas
~~~html
<!--html-->
<div class = "container">
    <header>header</header>
    <aside>aside</aside>
    <main>main</main>
    <div class = "ad">ad</div>
    <footer>footer</footer>
</div>
~~~

~~~css
.container {
    display:grid;
    grid-template-areas:
    "header header header"
    "aside mian ad"
    "footer footer footer"; /*如果要空一个，可以设一个随意的值，一般设一个(.),如下*/
    /*". footer footer";*/
}

header {
    grid-area:header;
}

aside {
    grid-area:aside;
}

...........

~~~

