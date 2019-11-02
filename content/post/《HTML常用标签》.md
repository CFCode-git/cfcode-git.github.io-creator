---
title: "《HTML常用标签》"
date: 2019-11-01T21:04:35+08:00
draft: false
---

# a 标签

```html
<!-- html -->
<a href="//google.com" target="_blank" download rel="noopener"> </a>
```

## 作用

跳转到**外部页面**  
跳转到**内部锚点**  
跳转到**电话/邮箱**

## 属性

**href**

href 属性指定 a 标签要跳转到的新页面，他的取值可以是:

<em>网址</em>

1. https://google.com
2. http://google.com
3. //google.com

可以是 https 也可以是 http，当采用无协议传输方式时，浏览器会自动采取 http 或 https。

<em>路径</em>

1. a/b/c
2. /a/b/c

当 href 指向某个路径时，a 标签实际访问的是 http 服务的根目录"/"（也就是这个网站相关文件的根目录）。

用"/"开头的即表示根目录，此时用的是绝对路径。

"./"的意思是在当前 html 所在的目录中找，这是一种相对路径。

<em>伪协议</em>

1. javascript:代码;
2. mailto:邮箱
3. tel:手机号

`javascript:代码;`
是 JS 的伪协议写法，用户点击 a 标签就执行该 JS 代码。

<em>ID</em>

"#xxx"

href 可以指向某个 ID，用户点击 a 标签页面就会跳转到该拥有该 ID 的标签所在的位置。

**target**

target 属性指定 a 标签的新页面在浏览器中的打开位置，他的取值有:

<em>内置属性值</em>

1. \_blank
2. \_top
3. \_parent
4. \_self（默认值）

\_blank 指导新页面在浏览器的新标签页中打开；  
\_top 指导新页面在最顶层的页面打开；  
\_parent 指导新页面在上一层的父级页面打开；  
\_self 指导新页面在当前页面打开。

<em>自定义属性值</em>

设置 target 属性为 iframe 的 name 属性，则新页面将在 iframe 中打开。  
设置多个 a 标签的 target 指向同一个值，可以让这些新页面都在同一个标签页中打开。

**download**

该属性的作用是下载 a 标签指向的那个页面。_但很多浏览器不支持。_

**rel=noopener**

# img 标签

```html
<!-- html -->
<img src="1.html" alt="这是一个错误的地址" width="100" />
```

## 作用

发出 get 请求，展示一张图片。

## img 标签属性

**src**

必要属性，指定图片的绝对地址或相对地址。

**alt**

alt 中的内容会在图片加载失败时显示。

**height/width**

分别指定图片的宽和高，最好二选一进行设置，否则图片会变形。

## 事件

**onload & onerror**

onload 在图片加载成功时触发，onerror 在图片加载失败时触发。

## 响应式图片

使图片适应屏幕尺寸/手机页面。

```css
/*css*/
img {
  max-width: 100%;
}
```

# table 标签

```html
<!-- html -->
<table>
  <thead>
    <tr>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th></th>
      <td></td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th></th>
      <td></td>
    </tr>
  </tfoot>
</table>
```

## 语义化标签

**thead**

代表表格的头部，通常放表格的标题、各项指标。

**tbody**

代表表格的数据。

**tfoot**

代表表格的尾部，通常用于汇总数据。

_语义化标签的顺序可以随意，浏览器会自动将 thead 放在表格的最上方，tfoot 放在表格的最下方。_

## 一般标签

**tr**

table row,代表表格中的一行。

**th**

table head,代表表头。

**td**

table data，代表表格中显示的数据。

## table 相关样式

```css
/* css */
table {
  table-layout: auto;
  table-collapse: collapse;
  table-spacing: 0;
}
```

**table-layout**

auto -- (默认)根据表格中的内容自动调整表格宽度。  
fixed -- 固定宽度，使表格等宽。

**border-collapse**

collapse:使表格中的边框合并。

**border-spacing**

0:表示单元格间没有空隙。

## table 合并单元格

**colspan & rowspan**

```html
<!--  html -->
<tr>
  <td colspan="2"></td>
  <td rowspan="2"></td>
  <td></td>
</tr>
<tr>
  <td></td>
  <td></td>
  <td></td>
</tr>
```

colspan 合并列，rowspan 合并行。

# iframe 标签

# form 标签

# input 标签

**作用**  
让用户输入内容

**属性**

<em>类型</em>  
button --按钮  
checkbox --多选框  
email --邮件  
file --上传文件  
hidden --隐藏  
number --数字  
password --密码框  
radio --单选框  
search --搜索框  
submit --提交按钮  
tel --电话号码  
text --单行文本框（默认值）  
color --选择颜色  
.......

关于 checkbox 和 radio，需要进行分组，同一组的 input 标签 name 属性一致。

<em>事件</em>

onfocus --当标签获取焦点时触发  
onchange --当标签中的内容发生变化时触发  
onblus --当标签失去焦点时触发

<em>验证器</em>

- _<b>注意事项</b>_
  1. 一般不监听 input 的 click 事件。
  2. form 里面的 input 要有 name。
  3. form 标签中要放一个 type = submit 才能触发 submit，表单才能提交。

# 其他输入标签

## select + option --选择框

```html
<!-- html -->
<select>
  <option value=""></option>
  <option value=""></option>
  <option value=""></option>
</select>
```

value 是实际传递的值，标签之间的内容是给用户看的值。

## textarea --多行文本框

```html
<!-- html -->
<textarea style="resize:none;"></textarea>
```

`style = "resize:none;"`是固定多行文本框的尺寸。

## label

未完待续...

_**版权声明：本文为周啟尧原创文章，著作权归本人和饥人谷所有，转载务必注明来源**_
