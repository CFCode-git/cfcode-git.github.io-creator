---
title: "如何用hugo搭建个人博客"
date: 2019-10-30T10:13:42+08:00
draft: false
---

# 如何用hugo搭建个人博客

以下是简略步骤：
1. hugo下载&安装
2. 建立新网站
3. 添加主题
4. 开始第一篇文章
5. 启动hugo服务器
6. 自定义主题
7. 建立静态页面
8. 将博客放到网上

## hugo下载&安装（windows）

1. 下载 hugo_xxxxx_Windows-64bit.zip；
2. 解压，将hugo.exe放到软件目录中；
3. 将hugo.exe的存放路径添加到系统高级配置的环境变量中；
4. 运行`hugo version`检查hugo.exe是否安装成功。

[hugo官网](https://gohugo.io/getting-started/installing)

[hugo下载地址](https://github.com/gohugoio/hugo/releases)

## 建立新网站

`hugo new site quickstart`

注：quickstart为你的博客仓库名。

### 添加主题

```
git init --创建一个git本地仓库

git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke --添加默认主题ananke

echo 'theme = "ananke"' >> config.toml --将默认主题添加到网站配置文件中
```
### 开始第一篇文章
`hugo new posts/my-first-post.md`
注意："my-first-post"为你的第一篇博客的名字。
在编辑器中编写内容：
```
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```
注：
* title 文章标题。
* date 日期可以改，但只能往前改。
* 提交前将 draft 的值改为：false，即本篇博客不是草稿。
### 启动hugo服务器
运行以下命令：
```
hugo server -D --可以预览草稿
（或hugo server --可以预览非草稿）
```
该命令创造一个可访问的地址，用来预览博客。（按ctrl+c关闭服务器后，将无法预览）
### 自定义主题
打开配置文件"config.toml"
```
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```
注：

* languageCode 为博客显示的语言，建议设置为简体中文："zh-Hans"。


* title 为整个博客网站的标题。


* 可以通过设置 theme 更换博客主题。

### 建立静态页面

运行代码`hugo`，创建一个新的目录"public"。

### 在github展示html
1. 新建文件.gitignore,忽略public目录。
2. 进入public目录，创建一个本地仓库并提交。
```
cd public
git init
git add .
git commit -v
```
3. 在github创建一个仓库，仓库名应该与博客仓库名对应(step2)

```
git remote add origin xxxxx --xxxxx为ssh地址
git push -u origin master
```
4. 进入github仓库 - setting - GitHub Pages -Source选项，选择master。
5. 点击 GitHub Pages 中的URL即可访问博客。

### *参考资料来源*
[hugo快速入门](https://gohugo.io/getting-started/quick-start/)

[写代码啦](https://xiedaimala.com/tasks/4750e955-a081-480b-9a77-c109e75eba23)