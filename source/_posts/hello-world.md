---
title: Ocean
---
更多Ocean [Ocean](https://zhwangart.github.io)

## Start

### 安装

``` bash
$ git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean
```

More info: [Ocean](https://zhwangart.github.io)

### 启用

在 root _config.yml 中选择 theme: ocean

``` bash
$ theme: ocean
```

More info: [Ocean](https://zhwangart.github.io/2018/11/30/Ocean/)

### 更新

``` bash
$ cd themes/ocean
$ git pull
```

More info: [Ocean](https://zhwangart.github.io)

### 配置

默认开启相册与关于菜单，关闭 Gitalk 评论功能，需要的同学 true 就可以了，关于 Gitalk 的使用 过程中遇到各种报错，有同样问题的，或者有兴趣想要了解 Gitalk 可以移步看一看
``` bash
# Menu
menu:
  主页: /
  归档: /archives
  相册: /gallery
  关于: /about
rss: /atom.xml

# Miscellaneous
favicon: /favicon.ico
brand: /images/hexo.svg

# Ocean 主页视频
# 多种格式的视频用于支持不同的浏览器，这里只需要配置好路径，前提是我把视频相关文件统一目录存放。
ocean:
  overlay: true      # 可选，false 则 Ocean 视频下方的笔触式遮盖不显示
  path: /images/ocean/      # 视频统一存放路径，格式 mp4/ogg/webm
  brand: /images/hexo-inverted.svg      # 可选，一个小 Logo

# 内容
excerpt_link: 阅读全文...
share_text: 分享
nav_text:
  page_prev: 上一页
  page_next: 下一页
  post_prev: 前一篇
  post_next: 后一篇

# fancybox
fancybox: true

# Local search
search_text: 搜索

# Gitalk
gitalk:
  enable: false      # 默认关闭评论 开启：true
  clientID:      # 申请 GitHub Application 网页上对应的 Client ID 与 Client Secret 参数
  clientSecret:      # 同上
  repo:      # 创建的仓库名称
  owner:      # Github ID
  admin:      # Github ID
```
Ocean 使用了 feathericon 图标库，菜单中的图标定义在“CSS source / css / _partial / navbar.styl”中，可根据需要进行更改或添加。
如果你不需要开启 相册 与 关于 菜单，需要删除或者注销掉他们的图标，如下边的示例：

``` bash
.nav-item
  &:nth-child(1)         // 主页
    .nav-item-link
      &::before
        content '\f12f'
  &:nth-child(2)         // 归档
    .nav-item-link
      &::before
        content '\f12a'
  //&:nth-child(3)         // 相册
  //  .nav-item-link
  //    &::before
  //      content '\f1a9'
  //&:nth-child(4)         // 关于
  //  .nav-item-link
  //    &::before
  //      content '\f174'
```
如果你想要开启 Tag（标签）、Categories（分类） 在菜单中显示，请看这里：[关于 Ocean 使用中的问题](https://zhwangart.github.io/2018/11/30/Ocean/)

## 插件

### hexo-generator-search 本地检索

``` bash
$ npm install hexo-generator-searchdb --save
```

Hexo 的配置文件 _config.yml 添加插件配置（注意：不是主题的配置文件）：

``` bash
search:
  path: search.xml
  field: post
  content: true
```

### hexo-generate-feed RSS

``` bash
$ npm install hexo-generator-feed --save
```

Hexo 的配置文件 _config.yml 添加插件配置（注意：不是主题的配置文件）：

``` bash
feed:
     type: atom
     path: atom.xml
     limit: 20
     hub:
     content:
     content_limit: 140
     content_limit_delim: ' '
     order_by: -date
```

### hexo-generator-index-pin-top 文章置顶

``` bash
$ npm uninstall hexo-generator-index --save
$ npm install hexo-generator-index-pin-top --save
```

在需要置顶的文章的 Front-matter 区域加上 top: ture ，示例：

``` bash
---
 title: 新增文章置顶
 author: zhwangart
 date: 2019-07-18 15:45:03
 top: ture
 ---
```

### 文章封面图

需要写在 markdown 的 Front-matter 区域：

``` bash
---
title: Post name
photos: [
        ["img_url"],
        ["img_url"]
        ]
---
```

需要注意的是，这里说的封面图并不是文章配图，文章配图按照 markdown 的语法写就好了！

### 相册

首先需要创建一个 page ，关于页面也一样需要创建.

``` bash
$ hexo new page gallery
```

然后在编辑 markdown 的时候需要写在 Front-matter 部分，这种写法可能不是特别特别的好，希望能有更好的方法。

``` bash
---
title: Gallery
albums: [
        ["img_url","img_caption"],
        ["img_url","img_caption"]
        ]
---
```

### Toc 文章目录

使用 Tocbot 解析内容中的标题标签 (h1~h6) 并插入目录。使用方法：
Ocean 配置 Toc 只作用于 Post 页面，在其他 Page 中不作用。
1. 在主题配置文件 ocean/_config.yml 中开启

``` bash
# Toc
toc: true
```

2. 在 Post 中关闭 Toc
如果在 ocean/_config.yml 开启了 Toc ，那么 Tocbot 会在每一篇 Blog 都解析内容中的标题标签生成 Toc 文章目录，但并不是所有的 Blog 中都需要 Toc，所以在 markdown 的 Front-matter 部分可以关闭：

``` bash
---
toc: false
---
```

### 部署到 Github

安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)

``` bash
$ npm install hexo-deployer-git --save
```

修改配置

``` bash
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch] #published
  message: [message]
```

## End



More info: [Ocean](https://zhwangart.github.io)
