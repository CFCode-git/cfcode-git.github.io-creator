---
title: "谈谈浏览器解析html时的阻塞问题"
date: 2019-11-25T13:33:43+08:00
draft: false
---

参考文章：    

[DOM阻塞总结](https://www.jianshu.com/p/8fd296377539)    
[深入浅出浏览器渲染原理](https://blog.fundebug.com/2019/01/03/understand-browser-rendering/)     
[浏览器是如何解析html的？](https://juejin.im/post/5c1dde33f265da61776bf49a)     
[](https://javascript.info/onload-ondomcontentloaded)     
[](https://www.zhihu.com/question/30218438)     
[](https://juejin.im/entry/5c6b629be51d457fa31e6a1d)     


# 浏览器是如何工作的?

当我们在地址栏输入一个合法的URL后，浏览器首先进行域名解析，得到服务器的IP地址，然后浏览器会给服务器发送GET请求，等到服务器响应200后开始下载并解析html。

页面主要由DOM、CSS、JavaScript等部分构成，其中CSS和JavaScript既能内联也能以脚本的形式引入，此外还有img、iframe等其他资源，所有的这些资源都是通过dom标签的形式嵌入在html页面中，接下来我们分析一下dom的构建过程。

# 浏览器的工作流程

> 构建DOM => 构建CSSOM => 构建渲染树(Rendering Tree) => 布局(也叫回流) => 绘制

大体来讲：   
1. 浏览器会解析三个东西：     
   * HTML/SVG/XHTML，产生DOM Tree。  
   * CSS，产生CSS Rule Tree。     
   * JavaScript，脚本，通过DOM API 和CSSOM API操作DOM和CSSOM。  
2. 解析完成后，结合DOM Tree和CSSOM Tree构造Rendering Tree。    
   * Rendering Tree和DOM Tree并不是完全相等的，比如Header和display：none就不会出现在Rendering Tree中。   
   * 将CSS Rule添加到Rendering Tree上的节点，然后计算每个元素的位置，这就是布局。    
3. 调用操作系统Native GUI的API绘制。

# DOM的构建过程

DOM的构建过程是顺序的，渐进式的。从第一行开始，逐行依次解析，并且会将已解析完成的部分显示出来。  

**如何判定DOM构建完成？**    

我们使用JavaScript操作DOM或者给DOM绑定事件的前提就是DOM树已经构建完成。当DOM树构建完成时，document对象会派发事件DOMContentLoaded来通知DOM树已经构建完成。

html从第一行开始解析，遇到外联资源(外联CSS、外联JS、image、iframe等)则会请求下载对应的资源。而其中有一部分会影响（阻塞）DOM的构建。

**正常情况下CSSOM和DOM的构建是互不干扰的**

浏览器下载HTML文件，解析HTML从而构建DOM。

遇到link[rel=stylesheet]时，将其加入下载队列，继续构建DOM。（CSS不会阻塞DOM的构建）

**而JS的加载、解析和执行都会阻塞DOM的构建**

当HTML解析器遇到了JavaScript，那么它会暂停构建DOM，将控制权移交给JavaScript引擎，等JavaScript引擎运行完毕，浏览器再从中断的地方恢复DOM构建。

**接下来具体分析。**


## CSSOM和DOM

CSSOM和DOM的构建互不干扰，那么所有的CSS都不影响DOM的构建吗？

不是，这是有前提的。

前提是这些CSS样式不被JavaScript需要。

我们知道，JavaScript不只是可以改DOM，它还可以更改样式，也就是它可以更改CSSOM。而不完整的CSSOM是无法使用的，因此如果JavaScript想访问CSSOM并更改它，必须要拿到完整的CSSOM，而JavaScript的加载、解析和执行都会阻塞DOM的构建，这样一来就导致了：   

某一时刻CSSOM和DOM并行下载（解析）着，突然遇到了JavaScript想要运行脚本，那么浏览器只能让JavaScript先等一等，然后优先下载和完成目前为止的CSSOM的构建，再执行JavaScript脚本，然后才继续进行DOM的解析。

**也就是说在JavaScript之前的CSS样式，准确的说，是被JavaScript依赖执行的CSS（内联CSS、以及JavaScript标签之前的外联CSS）,有可能会因为JavaScript的影响，间接阻塞了DOM的构建。**

JavaScript之后的CSS则不受影响。

> 所以，如果想首屏渲染更快，就不应该在首屏就加载JS文件，所以这也是一般建议把script标签放在body标签的底部。

## JavaScript和DOM

同样，JavaScript一定会阻塞DOM的构建吗？

不一定。

我们可以通过async属性和defer属性，降低JavaScript对DOM的阻塞。

对于普通的script标签`<script src = "script.js"></script>`，浏览器会立即加载并执行指定的脚本。读到就加载并执行，不等待后续载入的文档元素。

**异步下载：`<script async src = "script.js"></script>`**  

async 属性表示异步下载引入的JavaScript，此时JavaScript的下载不会影响DOM的构建，但一旦下载完成，不论D是在DOM的解析阶段还是在DOMContentLoaded事件触发之后，**但一定是在load事件触发之前，**JavaScript就会执行。

如果是在DOM解析阶段，JavaScript的执行会阻塞DOM的解析。如果是在DOMContentLoaded触发之后，JavaScript会阻塞load事件。


**延迟执行：`<script defer src = "script.js"></script>`**

defer 属性表示延迟执行引入的JavaScript。该属性的JavaScript加载时，并未阻塞HTML的解析，这两个过程是并行的。

直到整个document解析完毕且该JavaScript也加载完成后，才会执行所有由defer-script加载的JavaScript代码。**然后再触发DOMContentLoaded事件**

与普通的script相比，延迟执行区别在于：载入JavaScript文件时不会阻塞html的解析，JavaScript的执行被放到了html标签解析完成之后，DOMContentLoaded事件触发之前。

当加载多个JS脚本时，async是无序的加载，而defer是有序的加载。

用下图更直观地说明问题

蓝色部分表示JavaScript的加载（下载）    
灰色部分表示JavaScript的执行     
绿色部分表示html的解析     
![script](/images/DOM阻塞/dom阻塞4.png)

# 总结

* 浏览器的工作流程：构建DOM => 构建CSSOM => 构建渲染树(Rendering Tree) => 布局(也叫回流) => 绘制

* CSSOM会阻塞渲染，只有CSSOM构建完毕后才会进入构建渲染树的阶段。

* 通常情况下，CSSOM和DOM是并行构建的，但是当浏览器遇到script标签时，DOM构建将停止，直到脚本完成执行。      
同时由于JavaScript可以修改CSSOM，所以也必须等到CSSOM构建完成后才能执行JS。      
我们可以设置延迟执行和异步下载减少JavaScript对DOM构建的影响。

* 如果想首屏渲染更快，那么不应该在首屏就加载JS文件，建议将script标签放到body标签底部。 


阻塞DOM构建的有：       
* JavaScript标签之前的CSS       
* 外联普通JavaScript        
* 外联defer-JavaScript的执行过程           
* 内联JavaScript      

不会阻塞DOM构建的有：   
* JavaScript标签之后的CSS     
* 外联async-JavaScript     
* 外联defer-JavaScript的加载过程   
* image     
* iframe     


# 关于回流和重绘

当页面生成时，至少会渲染一次。而在用户访问的过程中，还会不断地重新渲染。

重新渲染可能是回流和重绘，也可能只有重绘。

* 重绘：当渲染树中的一些元素更新的属性只是影响元素外观，而不影响布局，就会触发重绘。

* 回流：当渲染树中的元素大小尺寸、布局等发生改变时，需要触发回流。

**回流必定会触发重绘，重绘不一定引发回流。**    
回流和重绘在我们进行节点样式设置时会频繁触发，这在很大程度上影响性能。

**如何减少重绘和回流？**

* 使用 transform 代替 top    
* 使用 visibility 代替 display:none ,因为前者只会引起重绘，后者会引发回流。（改变了布局）     
* 不要把节点的属性值放在一个循环里当成循环的变量（这样会频繁引发回流或重绘）   
* 不要使用 table 布局，因为一个很小的改动可能会造成整个 table 的重新布局。  
* 动画实现的速度选择，动画速度越快，回流次数越多。也可以使用requestAnimationFrame     
* CSS选择符从右往左匹配寻找，避免节点层级过多。    
* 将频繁重绘或回流的节点设置为图层，图层能够阻止该节点的渲染行为影响别的节点，比如对于video节点来说，浏览器会自动将该节点变为图层。 

# 为什么DOM操作慢？

因为浏览器是单线程的。DOM是属于渲染引擎负责的，而JS是JS引擎负责的，两者分属于不同的线程。我们通过JS操作DOM的时候，实际上涉及到了两个线程之间的通信，DOM操作所耗费的时间实际上就是两个线程通信所需要的时间。

# 渲染页面时常见的不良现象

* FOUS(无样式内容闪烁)：     

由于浏览器渲染机制，在CSS加载前，先呈现了HTML，因此会展示出无样式的内容，然后样式突然呈现的现象。

* 白屏

有些浏览器的渲染机制（比如chrome）要先构建DOM树和CSSOM树，构建完成再进行渲染。如果CSS部分放在HTML尾部，由于CSS未加载完成，浏览器迟迟未渲染，就会导致白屏。
也有可能是JS文件放在了头部，阻塞了后续DOM的构建和组件的下载，导致了白屏的产生。


