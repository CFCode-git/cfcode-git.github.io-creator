---
title: "JS对象"
date: 2019-11-16T14:41:49+08:00
draft: false
---

JavaScript的其中数据类型：四基两空一对象，今天我们来聊一聊对象Object，

以及原型是什么？

# 唯一的复杂类型——Object

~~~JavaScript
//常用写法：
let obj = {'name':'xxx','age':'xxx',}
//正规写法
let obj = new Object({'name':'xxx','age':'xxx'})
//匿名对象
console.log({'name':'xxx','age':'xxx'})
~~~

**注意：**  
1. 键名是字符串，可以包含任何字符；  
2. ''可以省略，但是省略后只能写标识符和数字；（所以为什么要省略它呢？写上！）  
3. 对象没有所谓的数字下标，要得到对象中的某个值，只能通过它的键名获取。    
4. **对象中的键值对又称为这个对象的属性。**  

~~~JavaScript
let obj={
    1e2:'一百'
}
/*******结果是********/
obj = {'100':'一百'}
~~~

**如果对象的键是一个表达式，那么JavaScript会将该表达式求值，并将结果转换为字符串。**  

## 提问：字符串是一个常量，我能不能用变量的值作为Object的键呢？
答案是肯定的。   
怎么做？    

用'[]'将变量名包裹起来 

~~~JavaScript
let a = 'xxx'
var obj = {
    [a] = 'hello'
}
/*******结果是********/
obj = {'xxx':'hello'}
~~~

**同上面表达式的原理类似，JavaScript会先得到变量的值，再转为字符串。**   

# Object中的增删查改

## 删除属性

delete obj.xxx 或 delete obj['xxx']  ——删除obj对象的xxx属性

**注意区分属性值为 undefined 和 不含属性名**    

比如

~~~JavaScript
obj.xxx = undefined
obj.xxx = null
~~~

上面的操作只是把obj对象中的xxx属性的属性值设置为null或undefined（相当于删除属性值）。

**但是！**，xxx属性还保留着，只是这个属性没有值，或者说它的值为null（或undefined）。


## 查看属性

1. 查看自身所有属性

~~~JavaScript
//查看对象中所有的键
Object.keys(obj)
//查看对象中所有的值
Object.values(obj)
//查看对象中所有的键值对
obj   
Object.entries(obj) /*以数组的形式展示*/
~~~

2. 查看自身属性和共有属性

~~~JavaScript
//打印出obj对象的所有属性
console.dir(obj)
~~~

上面是最佳方法，也可以用obj.__proto__逐个输出，不推荐。

3. 判断对象中是否含有某属性

~~~JavaScript
//判断对象中是否含有某个属性
'xxx' in obj /*返回true表示含有，false表示没有*/
//判断某个属性是不是自身属性
obj.hasOwnProperty('toString') /*返回true表示非共有，false表示这是共有属性*/
~~~

4. 查看具体某个属性

~~~JavaScript
obj.key | obj['key'] /*['key']中的''不能省略，省略后key表示的是变量。变量key的具体值是不确定的*/
~~~

## 修改 || 增加属性

如果对象中没有该属性则增加该属性，否则就修改该属性。

~~~JavaScript
//直接赋值
let obj = {'name':'hi'}
obj.name = 'hi'
obj['name'] = 'hi'
//批量赋值
Object.assign(obj,{'age':'18','gender':'man'})
~~~

# 共有属性和原型

1. 每个对象都有原型。    
2. 对象的原型本身也是对象。    
3. `obj={}`的原型是Object中的prototype属性，它是所有对象的原型，是所有原型的根，包含所有对象的共有属性。这个原型（prototype）本身的也有原型（`__proto__`），`__proto__`的值为null。

**怎么理解原型？**   

可以把原型看作是一个存储JavaScript各种函数功能的仓库，里面的共有属性就是函数，对象类似于某品牌的批发店，所有该品牌的批发店（对象）就是从这个仓库（原型）共享货源（属性）。

## 修改共有属性

一般无法通过自身修改或增加共有属性，比如：

~~~JavaScript
var obj = {}
obj.toString = 'xxx'
~~~

上面的代码不会修改obj原型的toString属性，而会在obj的底下新增一个名为toString的自身属性。**并且之后再也无法访问原型中的toString属性。**

**并且我们不推荐修改原型中的共有属性，因为这样会影响到整个代码，如果偏要修改：**

~~~JavaScript
obj.__proto__.toString = 'xxx'
Object.prototype.toString = 'xxx'
~~~

修改共有属性会破坏JavaScript的环境，这也正表明了JavaScript的脆弱性。

## 修改原型

相比于修改共有属性，我们更推荐**修改原型**。

比如：

~~~JavaScript
/*将obj对象的原型设置为空。*/
obj.__proto__ = null
~~~

比如：
![修改原型](/images/原型链.png)

> 原型链是个啥？   
> 承接上面的比喻，原型是一个仓库，共有属性是货品，对象是批发店和连锁超市，连锁超市从批发店得到货品，批发店的货品来自仓库，从仓库到批发店再到连锁超市就构成了供应链（原型链）。

**推荐用Object.create修改原型**

~~~JavaScript
var common = {'country':'China','haircolor':'black'}
var person = Object.create(common)
~~~

上述代码的意思是，以common为person的原型。运行后，common中的内容会成为person的隐藏属性，出现在`__proto__`中。

如果要给person添加属性，有两种方法：

~~~JavaScript
/*第一种方法*/
var person = Object.create(common,{
    'name':{value:'Jack'}
})
/*第二种方法*/
var person = Object.create(common)
person.name = 'Jack'
Object.assign(person,{...:...,...:...}) /*配合assign添加属性*/
~~~