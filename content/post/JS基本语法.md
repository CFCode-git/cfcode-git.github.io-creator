---
title: "JS基本语法"
date: 2019-11-15T06:40:40+08:00
draft: false
---

# 表达式和语句

## 表达式和语句的 *一般* 区别
1. 表达式一般有值，语句可能有或没有；  
2. 语句一般会改变环境（声明、赋值等）  
3. 上述区别并不绝对。  

**关于值和返回值**  
只有函数（表达式）才有返回值的概念。比如：  

1. `1+2`的值是3；  
2. `add(1,2）`表达式的值为函数的返回值  
3. `console.log`表达式的值为函数本身    
4. `console.log(3)`表达式的值为undefined（console.log没有返回值，打印出"3"是它的功能）   

## 注意事项

1. JavaScript中区分大小写；  
2. 大部分空格没有意义；  
3. 大部分回车没有意义（除了return，return后面不能随便回车，否则JavaScript会自动在return后面补上";"）

# 标识符

**命名规则**  
第一个字符后面可以接Unicode字符、"$"符号、"_"符号和中文，之后的字符除了这些之外，还可以是数字。


# 注释

多行注释 "/**/"

单行注释 "//"

**好的注释：**  
/*踩坑注解*/  
/*为什么代码写的这么奇怪，遇到了什么bug*/  

# 条件语句 if...else...

~~~
/************************/
if(表达式){
    语句1
}eles{
    语句2
}
/************************/
if(表达式){
    语句1
}eles if{
    语句2
}else{
    语句3
}
/************************/
function fn(){
    if(){
        return 表达式
    }
    if(){
        return 表达式
    }
    return 表达式
}
/************************/
~~~

**注意**  
1. "{}"不要省略，省略后if..else..语句只能作用到后面的第一个语句。

## 其他条件语句 

1. 三元表达式

~~~
表达式1 ? 表达式2 : 表达式3
~~~

如果表达式1为true则执行表达式2，否则执行表达式3。

2. switch语句

~~~
switch(表达式){
    case 取值1:
        执行语句
        break;
    case 取值2:
        执行语句
        break;
    default:
        执行语句
}
~~~

根据表达式的取值，选择下面的case执行。(_break不能省略，否则程序会一直按顺序执行直到出现break才跳出。_)

3. &&短路逻辑

~~~
A && B && C && D
~~~

A B C D 按顺序依次执行，**但是！**当执行中遇到某一个表达式为false，则就此打住，得到的是这个表达式的值。当全部都为true时，得到的是最后一个表达式D的值。

用法示例：  

~~~
console && console.log && console.log("hi")
~~~
IE并没有console，所以IE浏览器不会打印出"hi"

4. ||短路逻辑

~~~
A || B || C || D
~~~

A B C D 按顺序依次执行，**但是！**当执行中遇到某一个表达式为true，则就此打住，得到的是这个表达式的值。当全部都为false时，得到的是最后一个表达式D的值。

用法示例：  

~~~
a = a||100
~~~

**用于设定保底值**，上面的语句等价于：

~~~
if(a){
    a=a
}else{
    a=100
}
~~~


# 循环语句 

## while

~~~
while(表达式){
    语句块
}
~~~

当表达式成立时执行语句，注意在此之前要对判断的语句进行初始化，而在语句块中要对判断的变量进行增量计算。

**do...while...语句和 while 差不多，区别是do...while...语句先执行语句块再判断表达式，无论表达式是否成立，语句块都至少执行一遍。**

# for

~~~
for(语句1;表达式2;语句3){
    循环体；
}
~~~

先执行语句1（初始化）；  
判断表达式2；  
表达式2为真，执行循环体，再执行语句3；  
表达式2为假，退出for语句；  

示例：  
~~~
for(var i = 1; i < 5; i++){
    setTimeout( () => {
        console.log(i)
    },0)
}
~~~
结果会打印5个5.   

**为什么？**    

原因是setTimeout是定时器，过一会儿再打印，所以语句会先执行完for语句，再执行定时器。而当执行完for语句后，i的值就是5。因此会打印出5个5.  

**如果将 var 换成 let,会改变结果为"0 1 2 3 4"**  

# break 和 continue

break用于跳出**整个**循环；continue用于退出**本次**循环。

# label语句

~~~
/*语法*/
foo: {
    console.log(1);
    break foo;
    console.log('本行不会输出')
}
~~~

示例：  
~~~
{
    foo:1;
}
~~~
上面的语句**不是对象**，上面是一个代码块（block），代码块里是一个label标签foo，标签的值是1。

如果要声明为对象：

~~~
var obj = {
    foo:1;
}
~~~