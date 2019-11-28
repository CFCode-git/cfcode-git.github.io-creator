---
title: "DOM编程"
date: 2019-11-24T15:22:43+08:00
draft: true
---

# DOM是什么

MDN有很详细的描述

> * DOM 是文档对象模型，英文全称 Document Object Model，是 HTML 和 XML 文档的编程接口。它提供了对文档的结 构化表述，可以使程序可以对该结构进行访问，从而改变文档的结构、样式和内容。DOM将文档解析为一个由节点和对象（包含 属性和方法的对象）组成的结构集合。

> * DOM 遵循 W3C DOM 和 WHATWG DOM标准；

> * 许多浏览器上提供了对 W3C 标准的扩展，因此文档在不同的浏览器中会有不同的实现。

> * 所有操作和创建web页面的属性、方法和事件都会组织成对象的形式。

简单来理解，就是我们的网页是一种树结构。

比如下面这个网页

![html](/images/DOM编程/html树.png)

它的树结构如下（注意：文字也是一个节点）

![html](/images/DOM编程/html树2.png)

JS要如何操作这棵树？  
利用浏览器的document对象。  
在控制台中输入`window.document`，就可以得到文档的根节点。  
将网页"翻译"成一个对象；   
JS通过document对象操作网页，这个思想就是DOM。

# API

~~~
/***************************************************/


window.idxxx | idxxx（不能与全局属性冲突）
document.querySelector('#idxxx')
document.querySelectorAll('.red')[0]
/*如果要兼容IE：*/
document.getElementById('idxxx')
doucment.getElementsByTagName('div')[0]
document.GetElementByClassName('red')[0]

/*获取特定元素*/
document.documentElement //获取html元素
document.head //获取head元素
document.body //获取body元素
window //获取窗口（窗口不是元素）
document.all //获取所有元素（第6个falsy值）


/***************************************************/
~~~

# div的六层原型链

我们获取到的div是对象。

> 节点？元素？
>   
> 节点包含以下几种：
> `x.nodeType`  
> 1 表示元素Element，也叫标签Tag  
> 3 表示文本Text  
> 8 表示注释Comment  
> 9 表示文档Document  
> 11 表示文档片段DocumentFragment  
> **着重记住 1 和 3**  


`console.dir(div)`  
div => HTMLDivElement => HTMLElement => Element => Node => EventTarget => Object  

自身属性：className、id、style    

第一层原型：HTMLDivElement.prototype   
【所有div的共有属性】

第二层原型：HTMLElement.prototype    
【所有HTML标签的共有属性】

第三层原型：Element.prototype    
【所有XML、HTML标签的共有属性】

第四层原型：Node.prototype     
【所有节点的共有属性，节点包括XML标签文本注释、HTML标签文本注释等等】

第五层原型：EventTarget.property   
【里面最重要的函数属性是 addEventListener 】  

第六层原型：Object.property  


![div原型链](/images/DOM编程/div原型链.png)


# 节点的增删改查

## 创建节点

* **创建一个标签节点**

~~~
/***************************************************/

let div1 = document.createElement('div')
document.createElement('style')
document.createElement('script')
document.createElement('li')

/***************************************************/
~~~



* **创建一个文本节点**

~~~
/***************************************************/

text1 = document.createTextNode('你好')

/***************************************************/
~~~

* **标签里面插入文本**

~~~
/***************************************************/

// 标签里面插入文本
div1.appendChild(text1)
div1.innerText = '你好' | div1.textContent = '你好'
/**   但是不能用 div1.appendChild('你好')   **/

/***************************************************/
~~~

* **把 div 插入到 head 或者 body 中**

~~~
/***************************************************/

// 创建的 div 默认处于 JS 线程中
// 必须要把 div 插入到 head 或者 body 中才能生效
document.body.appendChild(div)
// 或者
已在页面的元素.appendChild(div)

/***************************************************/
~~~

> 问题：页面中有 div#test1 和 div#test2  
> let div = document.createElement('div')  
> test1.appendChild(div)  
> test2.appendChild(div)  
> 
> 问：最终 div 出现在哪？   
> 1. test1 里面  
> 2. test2 里面 ———— √  
> 3. 两个都有  
> 
> test2 里面；  
> 一个元素不能出现在两个地方，  
> 除非复制一份 ————>   
> `let div2 = div.cloneNode([deep])`  

**"deep"是否采用深拷贝（可选参数），如果为true，该节点和所有后代节点都会被克隆，否则只克隆节点本身**

## 删除节点
~~~
/***************************************************/
// 两个办法
// 新方法—— IE 不支持
childNode.remove()
// 旧方法
parentNode.childChild(childNode)

/***************************************************/
~~~

上述两个操作只是将节点移出页面（DOM树）  
还能再添加回来  
如何彻底删除？  
移出页面后，`childNode = null`

## 更改节点

### 更改
~~~
/***************************************************/

// 改 id
div.id =  'idname'

// 改 class ** class是保留字，所以不能用class **
div.className = 'blue red' //(全覆盖)
div.className += ' red' //添加class = ‘red’ 
// 改 class
div.classList.add('red') //添加class = ‘red’ 

// 改 style
div.style = 'width:100px; color:blue;' // 会覆盖之前全部的样式
// 改 style 的一部分
div.style.width = '200px' 

// 大小写——因为 JS 不支持中划线
div.style.backgroundColor = 'white'
div.style['background-color'] = 'white'

// 加自定义属性
div.setAttribute('data-x','test') // 自定义属性名data-x,属性值test
// 获取自定义属性
div.getAttribute('data-x')
// 如果自定义属性是以 data- 开头，就可以用这种形式
div.dataset.x 

// 改 data-*属性 
div.dataset.x = 'frank'

/***************************************************/

// 改内容
// 改文本内容
div.innerText = 'xxx'
div.textContent = 'xxx'

// 改 HTML 内容
div.innerHTML = '<strong>重要内容</strong>'

// 改标签
div.innerHTML = '' // 先清空
div.appendChild(div2) // 再加内容

// 改爸爸
newParent.appendChild(div)

/***************************************************/
~~~

### 读取
~~~
/***************************************************/
// 读标准属性
div.classList 
div.getAttribute('class') 
// 两种方法都可以，但值不同

// 对于a标签
a.href  // 都相对路径的时候会 *加上域名* ，返回一个原原本本的路径
a.getAttritube('href') // 是什么就读到什么

// 改事件处理函数（on开头属性）

// div.onclick 默认为 null
// 默认点击 div 不会有任何事情发生
// 如果把 div.onclick 改为一个函数fn
// 那么点击 div 的时候，浏览器会调用fn
// 并且调用方式为 fn.call(div,event)
// div 会被当作 this 
// event 则包含了点击事件的所有信息，比如坐标
test.onclick = function(x){
    console.log(this)
    console.log(x)
}
// this.onclick.call(test,event)

// div.addEventListener 是 div.onclick 的升级版
/***************************************************/
~~~

## 查看节点
~~~
/***************************************************/

// 查爸爸
node.parentNode | node.parentElement

// 查爷爷
node.parentNode.parentNode

// 查子代 【当子代变化时会实时变化，querySelectorAll 不会实时更新】
node.childNodes  // 包括文本节点，包括回车、空格，
node.children 

// 查兄弟姐妹 【要 排除自己】
node.parentNode.children
node.parentNode.childNodes // 【除了排除 自己 还要排除 文本节点】

// -----

// 查看大佬
node.firstChild

// 查看老幺
node.lastChild

// 查看上一个哥哥姐姐
node.previousSibling // 【会有文本节点】
node.previousElementSibling // 【避开文本节点】

// 查看下一个弟弟妹妹
node.nextSibling // 【会有文本节点】
node.nextElementSibling // 【避开文本节点】

//---------

// 遍历一个 div 里面的所有元素
travel = (node,fn) =>{
    fn(node) // 比如 fn 是 console.log ,先把当前节点打印出来
    if(node.children){
        for(let i = 0; i < node.children.length; i++){
            travel(node.children[i]，fn)
        }
    }
}

travel(div1,(node) => {console.log(node)})

/***************************************************/
~~~

# DOM是跨线程操作

> 为什么DOM操作慢？  
> 浏览器分为渲染引擎和JS引擎,DOM是跨线程操作。
> 

**跨线程操作**   
各线程各司其职  
JS引擎 不能操作界面，只能操作 JS。  
渲染引擎不能操作JS，只能操作页面。  
document.body.appendChild(div1) 这句 JS 是如何改变页面的？    

**跨线程通信**  
当浏览器发现 JS 在 body 里面加了一个 div1 对象。  
浏览器会通知渲染引擎在页面里也新增一个 div 元素。  
新增的 div 元素所有属性都照抄 div1 对象。  


**插入新标签的完整过程**

- 在 div 放入页面前

    对 div 进行的所有操作都属于线程内的操作。

- 在 div 放入页面时

    浏览器发现 JS 的意图。

    通知渲染线程在页面中渲染 div 对应的元素。

- 在 div 放入页面后

    对 div 的操作都有可能触发重新渲染。

    `div.id = 'newID'` 可能会触发重新渲染（如果 id 有 css 样式），也可能不会。

    `div.title = 'new'` 可能会触发重新渲染，也可能不会。

    如果连续对 div 进行多次操作，浏览器 可能会合并成一次操作，也可能不会。

- 如何避免合并？在操作中间加一些额外的操作，比如获取元素更改前的长度。


**属性同步**

* 标准属性 和 data-*属性  
    对 div 的标准属性的修改，会被浏览器同步到页面中；
    比如id、className、title。
* 非标准属性  
    对非标准属性的修改，则只会停留在 JS 线程中，不会同步到页面，比如x属性。

    **如果有自定义属性，又想同步到页面，应使用 data- 前缀**


## Property VS Attribute

* Property 属性  
    JS 线程中 div1 的所有属性，叫做 div1 的property

* Attribute  
    渲染引擎中 div1 对应标签的属性  

**区别**

大部分时候，同名的 Property 和 Attribute 值相等；  
但如果不是标准属性，那么他俩只会在 **一开始** 时相等；  
但注意 Attribute 只支持字符串；  
Property 支持字符串、布尔等类型；  
