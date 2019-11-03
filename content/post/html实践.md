---
title: "Html实践"
date: 2019-11-03T13:42:58+08:00
draft: false
---

# 选题 --我最喜欢的动漫《某科学的超电磁炮》

以动漫《某科学的超电磁炮》为主题制作一个简易介绍网页，html裸奔。

## 收集内容

资料来源：

[维基百科](https://zh.wikipedia.org/wiki/%E7%A7%91%E5%AD%B8%E8%B6%85%E9%9B%BB%E7%A3%81%E7%A0%B2)

[萌娘百科](https://zh.moegirl.org/zh-hant/%E6%9F%90%E7%A7%91%E5%AD%A6%E7%9A%84%E8%B6%85%E7%94%B5%E7%A3%81%E7%82%AE)

## ToC --Table of Content

为网页添加页内导航（锚点）  
实现手段：为每一个标题添加id，通过a标签的href指向这个id。

~~~html
<nav>
    <ol>
        <li><a href ="#introduce">作品介绍</a></li>
    </ol>
</nav>

<h2 id ="introduce">作品介绍</h2>
~~~

## 添加外部资源

添加图片，注意：  

* *设置图片大小统一，不要让图片变形*  
width/height二选一设置相同的值。

* *图片比例不对的要进行裁剪*

* *图片体积过大要进行压缩，一般不超过300kb*

* *添加图片预览*  
通过a标签超链接到原图，target设置为"_blank"。

## 兼容手机

* meta:vp
~~~html
<!--meta:vp-->
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
~~~

* 设置图片最大宽度为100%
~~~css
img {
    max-width:100%;
}
~~~

## 手机调试

* 通过chrome控制台

* wifi调试

让手机和电脑处于同一wifi，手机直接用ip和端口访问页面。
注：  
如果手机和电脑不在同一wifi，可以用映射端口，在chrome设置 > more tools > remote devices > Port forwarding 中设置。

* chrome远程调试

[chrome远程调试](https://developers.google.com/web/tools/chrome-devtools/remote-debugging/?hl=zh-cn)  

*忽略csdn搜索结果，google搜索"chrome远程调试 -csdn"*

## 部署到GitHub

在项目文件路径下运行：  

~~~
git init
git status
git add .
git commit -v
git remote add origin **地址**
git push -u origin master
~~~

在GitHub远程仓库中点击settings，找到GitHub page，选择分支为"master branch",刷新。  
点击GitHub page下的url，添加页面html的路径。  

**重要：复制网页地址，在GitHub仓库下点击"edit"添加预览地址。**

