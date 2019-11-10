---
title: "css知识总结"
date: 2019-11-10T09:50:38+08:00
draft: false
---


# css动画

## 浏览器渲染原理
1. 根据HTML构建HTML树（DOM）
2. 根据CSS构建CSS树（CSSOM）
3. 将两棵树合并成一棵渲染树（render tree）
4. Layout布局(文档流、盒模型、计算元素的大小和位置)
5. Paint绘制（边框颜色、字体颜色等...）
6. Composite合成（根据层叠关系展示画面）

## 三种更新方式
在实际操作中我们一般采用JS更新样式
~~~JavaScript
div.style.background ="red" 
div.style.display = "none" 
div.classList.add('xxx') --直接给元素添加一个"xxx"的类更好，该元素会拥有该类的所有属性
div.remove() --直接删掉节点
~~~
* JS/CSS > 样式 > 布局 > 绘制 > 合成 (比如div.remove()，导致文档流的其他元素重新布局。)
* JS/CSS > 样式 > 绘制 > 合成 (比如更改背景色，则布局不需要改变)
* JS/CSS > 样式 > 合成 (比如只改了transform)

**查询哪个属性触发哪个流程：**
[CSS Triggers](https://csstriggers.com/)

## css动画的两种做法
* 利用transition+transform+hover
* animation

### transform
transform的四个常用功能：  
1. 位移translate  
2. 缩放scale  
3. 旋转rotate  
4. 倾斜skew  
**注意：inline元素不支持transform，要先变成block.**

### transition
transition:属性名 时长 过渡方式 延迟；  
1. 可以用逗号分隔两个不同的属性；  
2. 可以用all表示所有属性；  
3. 过渡方式：linear | ease | ease-in | ease-out | ease-in-out | ...  

### 如何过渡两次？如何添加中间点？
1. 使用两次transform，用setTimeout监听transitioned事件。
2. 使用animation

### animation语法
animation：时长 | 过渡方式 | 延迟 | 次数 | 方向 | 填充方式 | 是否暂停 | 动画名  

时长：1s或1000ms；  
过渡方式：参考transition的过渡方式（ linear | ease... ）；    
次数：3 或 2.3 或 **infinite（无限）**  
方向：alternate（交替） | alternate-reverse | reverse  
填充方式：forwards（令动画停在最后一帧）| backwards | both | none  
是否暂停：paused | running  
~~~javascript
stop.onclick =()=>{
    demo.style.animationPlayState="paused"  ---为动画加上暂停按钮
}
~~~

### animation使用步骤

1. 声明关键帧
~~~css
/*css*/
@keyframes xxx{             ---xxx是动画名
    0%{transform:none;}     ---这是一个关键帧：0%表示初始状态
    50%{transform:...}
    100{transform:...}      ---一共有三个关键帧。
}
~~~

2. 添加动画

~~~css
/*css*/
#demo.start{
    animation:xxx 1s;            ---给demo的start属性添加动画。
}
~~~
~~~javascript
button.onclick =()=>{
    demo.classList.add('start')  ---点击按钮，则给demo添加一个start属性。
}
~~~

# css定位
## 回顾：css盒模型
[css盒模型](../css盒模型)

## position

* static(默认值)  
* relative(相对定位，升起来，但不脱离文档流)   
* absolute(绝对定位,定位基准是离他最近的祖先元素中的非static元素)   
* fixed(固定定位，定位基准是视口viewport)   
* sticky(粘滞定位)   

### position:relative
相对于自己定位，元素原来的位置不会消失。可用来做位移，或是做absolute元素的祖先元素。    
注意：  
1. 可以配合Z-index使用。  
2. 不要写Z-index:9999，要管理Z-index。    


### position:absolute
相对于祖先元素中的离他最近的定位元素定位。(定位元素就是position取值不是默认值static的元素)，可用做鼠标提示、对话框的关闭按钮。      
注意：  
1. 某些浏览器上不写top/left会位置错乱。  
2. 善用left：100%。  
3. 善用left：50%，加负margin。  

### position:fixed
相对视口定位，常用于广告、"回到顶部"按钮。  
注意：  
1. 手机上尽量不要使用这个属性，会引发很多问题。 

## 重要概念：层叠上下文
定位元素：position取值为非static的元素就是的定位元素，定位元素会跑到所有元素的外面。  

层叠上下文：默认的层叠上下文是html，可以在层叠上下文中创建层叠上下文。  
层叠上下文中的Z-index与外界无关，只用处于同一层叠上下文Z-index才能比较。（想象在html中创建许多小的"html"）  

**层叠上下文中定位元素的Z-index可以无限大、无限小，但他永远被层叠上下文包着。**

### 哪些不正交的属性可以创建层叠上下文？
MDN文档中有具体描述，优先记住以下常用的属性。  
1. 根元素  
2. Z-index值不为auto的绝对/相对定位  
3. opacity不为1的元素  
4. transform属性不为none的元素  
5. position：fixed  
6. Z-index的值不为"auto"的flex项目，即父元素display:flex | inline-flex  

# css基础
## 回忆：  
css的版本(css2.1、css3.0)

css语法(选择器、at语法)  

如何调试(border调试法)  

文档流(流动方向 宽度 高度 inline-block block inline元素的特点)  

脱离文档流(float和position的两个属性值)  

溢出(overflow)  

盒模型(border-box和content-box)  

margin合并(如何阻止合并?)

基本单位  

[回顾：css基础](../css基础.md)

# css布局
## 回忆：
布局思路(流程图)  

float布局(两个步骤：子元素float和父元素clearfix)  

flex布局(container和item)  

grid布局(container和item，**超牛x的属性：grid-template-areas**)

[回顾：css布局](../css布局.md)  

来这里复习一下：  
[青蛙游戏--flex :)](https://flexboxfroggy.com/#zh-cn)  
[天天种菜--grid :)](https://cssgridgarden.com/#zh-cn)



