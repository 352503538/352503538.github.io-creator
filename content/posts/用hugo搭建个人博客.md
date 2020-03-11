---
title: "用Hugo搭建个人博客"
date: 2020-03-08T13:45:21-07:00
draft: false
---
# 什么是hugo & 为什么要用hugo
Hugo是一个用`Go`语言编写的静态网站生成器。静态站点的好处就是快速、安全、易于部署，最主要是可以通过版本控制来进行管理。

# 如何用Hugo搭建个人博客

## 下载和安装Hugo

* MacOS 用户
首先安装 [Homebrew](https://brew.sh/) 然后在终端中运行
`brew install hugo`
`hugo version`

* Windows 用户
前往 [github的hugo/releases](https://github.com/gohugoio/hugo/releases) 下载适合自己系统版本的zip打包文件，然后解压到自己本地的博客目录，比如`D:\Hugo` 得到文件`D:\Hugo\hugo.exe`
将`D:\Hugo`加入到系统环境变量中，打开终端并运行`hugo version`

## 生成个人站点
在D盘下创建blog文件夹作为存放个人站点文件的主目录，在终端中运行：
```
cd D:\blog\
hugo new site 352503538.github.io-creator
```
之后在blog目录下会生成一个*352503538.github.io-creator*文件夹，其中archetypes目录里可以放一些原型，用于hugo新建内容的配置属性。config.toml是网站的配置属性文件。content文件夹里放你网站的内容，例如你发布的博客文章。data目录是Hugo使用的配置文件存放的地方。layout目录存放布局内容。static目录存放静态资源如图片、css等。

接下来我们可以准备编写第一篇文章了。命令如下：
```
cd 352503538.github.io-creator
hugo new post/my-first-blog.md
```
Hugo会在content目录下创建post目录，在post目录下创建`my-first-blog.md`文件。之后打开md文件，里面已经有些内容

```
---
title: "My First Blog"
date: 2020-03-05T19:14:50-08:00
draft: false
---
```
其中`---`中间的内容是TOML配置信息，其下方可以编写个人的博客内容，如果想要发布将`draft: true`改为`draft: false`即可。

如果觉得个人博客的主题太过单调，可以在[Hugo theme](https://themes.gohugo.io/)中添加自己喜欢的主题装饰一下。

我的主题是[MemE](https://github.com/reuixiy/hugo-theme-meme/blob/master/README.zh-cn.md)，这是由一个中国人编写的，简约且优雅，教程也比较清晰，喜欢的话可以自行添加。

## 本地测试
在终端中运行：
```
hugo server -D
```
在终端中点击 `http://localhost:1313/` 就能查看个人博客了（不过只能在本地中查看）。

## 上传到Github
首先在*352503538.github.io-creator*文件夹中新建 *.gitignore* 文件，然后在其中输入`/public/`。

然后在终端中运行：
```
hugo server
```

Hugo将编译所有文件并输出到`public`目录，你需要在github上创建repository，名字就是username.github.io，创建完后在终端中运行：
```
cd public
git init
git add .
git commit -m "第一次部署"
git remote add origin git@github.com:352503538/352503538.github.io.git
git push -u origin master
```
稍等片刻后，打开username.github.io网址，就可以看到自己的个人网站了。


