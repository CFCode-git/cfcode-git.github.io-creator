---
title: "JQuery中的函数"
date: 2019-11-30T18:52:19+08:00
draft: false
---

**_版权声明：本文为周啟尧原创文章，著作权归本人和饥人谷所有，转载务必注明来源_**

**_---参考资料来源：_**

[jQuery设计思想---阮一峰](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)

[jQuery中文文档](https://www.jquery123.com/)

---

`jQuery(选择器)`用于获取对应的元素，但是它不返回对应的元素，  
他返回一个对象，这个对象可以操作对应的元素。

# jQuery获取页面元素

利用构造函数jQuery()（或者它的别称$），传入一个css选择器作为参数。

~~~
/*选取整个文档对象*/
$(document) 

/*选取id为xxx的网页元素*/
$('#xxx')

/*选取class为xxxx的div元素*/
$('div.xxxx')

/*选取name属性为first的input元素*/
$('input[name=first]')
~~~

也可以传入jQuery特有的选择器

~~~
/*选择网页第一个a元素*/
$('a:first')

/*选择表格的奇数行*/
$('tr:odd')

/*选择表单中的input元素*/
$('#myForm :input')

/*选择可见的div元素*/
$('div:visible')

/*选择所有的div元素，除了前三个*/
$('div:gt(2)')

/*选择处于动画状态的div元素*/
$('div:animated')
~~~

# 链式操作

jQuery中的链式操作是通过jQuery对象调用jQuery函数，再次返回经过函数处理的jQuery对象实现的。

比如：

~~~
$('div').find('h3').eq(2).html('Hello');
~~~

`$('div')`返回一个对象，对象可以操作页面中的所有div；     
`.find('h3')`返回一个对象,对象操作上述div对象中的所有h3元素；    
`.eq(2)`返回一个对象，对象只操作上述h3元素的第三个；    
`.html('Hello')`返回一个对象，该对象是将上述h3元素的内容更改为hello后的结果。  

**可以通过`.end()`回退一步。**

**可以通过链式操作对jQuery选择的元素进行更进一步的精确选择**



~~~
// 选择包含p元素的div元素
$('div').has('p');

// 选择class不等于myclass的div元素
$('div').not('.myclass');

// 选择class等于myclass的div元素
$('div').filter('.myclass');

// 选择第一个div元素
$('div').first();

// 选择第六个div元素
$('div').eq(5);

// 选择div元素后面的第一个p元素
$('div').next('p')

// 选择div元素的父元素
$('div').parent()

// 选择离div最近的form父元素
$('div').closest('form')

// 选择div所有子元素
$('div').children()

// 选择div所有兄弟元素
$('div').sibling()
~~~

# 创建、删除和复制元素

复制元素用`.clone()`方法

删除元素用`.detach()`和`.remove()`  
前者会保留删除元素的事件，以便重新插入文档；后者则不保留

清空元素内容(但不删除元素)用`.empty()`

创建元素直接将新元素节点作为$（jQuery）的参数传入

~~~
$('<div>abcdefg</div>')
$('<li class = 'new'>new list item</li>')
$('ul').append('<li>list item</li>') // 将li节点插入到ul中
~~~

# 移动元素

有两种方法：  
1. 直接移动该元素   
2. 移动其他元素，使得目标元素到达我们想要的位置    

~~~
.insertAfter() 和 .after()  // 从现存的元素外部，从后面插入元素
.insertBefore() 和 .before()  // 从现存的元素外部，从前面插入元素
.appenTo() 和 .append()  // 从现存的元素内部，从后面插入元素
.prependTo() 和 .prepend()  // 从现存的元素内部，从前面插入元素
~~~

现在假设我们要把div元素移动到p元素后面，我们可以：

~~~
// 把div元素移动到p元素后面
$('div').insertAfter($('p');)

$('p').after($('div'))
~~~

# 修改元素属性

jQuery采用**重载**的设计模式，通过传入参数的个数，判断取值与赋值。

~~~
.html(?) // 如果有参数就设置元素的html内容，没有参数就去除html的内容
.text(?) // 取出或设置text内容
.attr(?) // 取出或设置某个属性的值
.width(?) // 取出或设置某个元素的宽度
.height(?) // 取出或设置某个元素的高度
.val(?) // 取出某个表单的值
~~~

**如果结果集（也就是jQuery对象）里面 包含多个元素，那么赋值的时候会对所有的元素进行赋值；**   
**但是取值的时候，只会取出第一个元素的值（.text()例外，它取出所有元素的text内容）**
