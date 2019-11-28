---
title: "Let和const在for循环中的运用"
date: 2019-11-27T06:12:07+08:00
draft: false
---

**_版权声明：本文为周啟尧原创文章，著作权归本人所有，转载务必注明来源_**

**_参考资料来源：_**

(let const缓存for循环的中间变量)[https://juejin.im/post/5c6bb8566fb9a049f154c363]

(let 和 const 在for 循环中的使用)[https://www.cnblogs.com/SamWeb/p/10659352.html]


问题的根源起源于手写jQuery的过程中用到的代码：

~~~JavaScript
find(selector) {
        let array = []
        for (let i = 0; i < elements.length; i++) {
            const elements2 = Array.from(elements[i].querySelectorAll(selector))
            array = array.concat(elements2)
        }
        array.oldApi = this
        return jQuery(array)
    }
~~~

const关键字定义的变量不能被重新赋值，那它为什么可以出现在for循环中？

------

# let

当我们在for循环中使用let时，每一次的迭代都会重新声明一次变量。比如`for(let i = 0; i < 10; i++)`

i变量被声明了10次，**每一次赋的值为上一次迭代完成时的值**，这样的话，循环体内获取到的i，每次也都是全新的变量i，而不是像使用var声明时得到的是全局变量，并且每一次迭代完成后，i变量就消失了。

除了常规for循环，for-in和for-of同理。每一次迭代都是重新声明一个新的迭代对象，而不是给原来的迭代对象赋新值。

~~~JavaScript
let arr = [1, 2, 3];
for (let key in arr) {
    console.log(key)
} // 0 1 2

for(let key of arr){
    console.log(key)
} // 1 2 3

// for in 遍历数组的索引（键名）；for of 遍历数组元素值
~~~

for of 与for in 的区别

# const

如果把上述代码中的let改成const，即：

~~~javascript
for(const i = 0; i < 10; i++)

for(const key in arr)

for(const key of arr)
~~~

可以发现，第一种常规for循环报错，而其他两种正常进行。

为什么？

看常规for循环的第一次迭代，const声明了一个变量i,在第一次迭代的最后尝试进行i++，因此报错。

而for-of和for-in没有问题，同let一样，每一次迭代都会声明一个全新的key，所有的赋值都是对一个全新的key **(变量)** 赋值，因此不会报错。

那么回答第一个问题，为什么for循环中可以用

` const elements2 = Array.from(elements[i].querySelectorAll(selector))`

因为每一次循环都是重新声明一个elements2变量对他进行赋值，const elements2 的作用域在for循环内，只要在**for循环内不进行二次赋值**，那就是可以的。

