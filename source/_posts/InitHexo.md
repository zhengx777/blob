---
title: 使用Hexo搭建Github博客
date: 2020-07-10 18:25:39
tags:
---
### 1. 新建仓库

首先打开github，点击New repository，创建一个新仓库，仓库名必须要遵守格式：账户名.github.io，不然接下来会有很多麻烦 ,此仓库是用来部署Hexo编译好的静态页。

![](/images/InitHexo/newRepository.png)


### 2. 本机启动Hexo服务

需要运行环境
- Node.js (Should be at least Node.js 8.10, recommends 10.0 or higher)
- Git

安装`hexo-cli`

``` bash
$ npm install -g hexo-cli
```

使用`hexo-cli`新建工程，并安装依赖。

``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

本地运行Hexo

``` bash
$ hexo s
```

服务启动后可以通过 http://localhost:4000 本地访问Hexo服务。

### 3. 部署

更改根目录下`_config.yml`配置文件`deploy`属性，做如下配置。

```
deploy:
  type: git
  repo: <repository url> # https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
```

安装`hexo-deployer-git`

``` bash
$ npm install hexo-deployer-git --save
```

运行`hexo clean && hexo deploy`将静态页部署到github, 即可通过`账户名.github.io`访问到博客。

``` bash
$ hexo clean && hexo deploy
```

### 常用Tip

- 在文章中插入图片

当Hexo项目中只用到少量图片时，可以将图片统一放在source/images文件夹中，通过markdown语法访问它们。

```
source/images/image.jpg
![](/images/image.jpg)
```



### 参考资料

1. 官方文档: [https://hexo.io/docs/](https://hexo.io/docs/)
2. 使用hexo搭建github博客: [https://www.jianshu.com/p/1bcad7700c46](https://www.jianshu.com/p/1bcad7700c46)
3. Hexo博客搭建之在文章中插入图片: [https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/](https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)


