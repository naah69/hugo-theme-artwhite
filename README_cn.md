# Art White Theme for Hugo

# [English Doc](https://github.com/naah69/hugo-theme-artwhite/blob/master/README.md)

Art White是Hugo的一款博客主题. 这是 [示例网站](http://www.naah69.com) .

它基于 [Clean White](https://github.com/zhaohuabing/hugo-theme-cleanwhite)主题进行开发.

## 1 新功能
1. 畅言评论
2. Algolia英语分词(需要node.js环境)
3. Algolia中文分词(需要java环境)
4. 不蒜子网页计数器
5. 文章浮动目录
6. 侧边栏标签
7. 页码按钮
8. 服务、编译、部署脚本

## 2 截图

### 2.1 首页

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/fullscreenshot.png)

### 2.2 文章页

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/post.png)

### 2.3 搜索页

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/search.png)

## 3 快速开始
1.进入Hugo项目的目录运行下面语句:

```
$ mkdir themes
$ cd themes
$ git clone https://github.com/naah69/hugo-theme-cleanwhite.git
```

 如果你的站点已经是一个git仓库，可能你想建立一个子模块，避免弄乱git仓库的话，运行下面语句.

```
$ mkdir themes
$ git submodule add https://github.com/naah69/hugo-theme-cleanwhite.git themes/hugo-theme-cleanwhite
```

2.把themes/hugo-theme-cleanwhite/requiredFile下的所有文件覆盖到你的项目目录中.

3.运行hugo本地服务命令
```
$ ./server
```
现在用浏览器进入 [`http://localhost:1313`](http://localhost:1313)


## 4 配置
首先让我们看看 [config.toml](https://github.com/naah69/hugo-theme-artwhite/blob/master/requiredFile/config.toml)。了解如何自定义你的网站，随意配置这些配置项。

### 4.1 畅言评论
我们使用的是[畅言](https://changyan.kuaizhan.com/)提供的评论服务，如果你想开启评论，去畅言创建一个账号，并在配置文件中填写以下配置。
```toml
#畅言评论配置
#changyan comment config
changyan_enable = true
changyan_appid = ""
changyan_conf = ""
```
你也可以通过changyan_enable来关闭评论。

### 4.2 Algolia全站搜索
1.跟着 [这篇教程](https://forestry.io/blog/search-with-algolia-in-hugo/#3-create-your-index-in-algolia) 在Algolia中创建你的索引.
我们需要将网站的索引存在网上。ArtWhite主题的搜索页面将对这个网站的插件进行适配

2.进入hugo项目的目录，运行下面的命令,这条命令需要 **node.js** 环境:
```bash
$ npm install hugo-algolia -g
```

3.然后我们创建一个叫**config.yaml**的文件, 输入下面的配置:
```yaml
---
baseurl: "your baseurl"
DefaultContentLanguage: "zh-cn"
hasCJKLanguage: true
languageCode: "zh-cn"
title: "your site title"
theme: "hugo-theme-cleanwhite"
metaDataFormat: "yaml"
algolia:
  index: "your algolia index"
  key: "your algolia admin key"
  appID: "your algolia appID"
---
```

4.在config.toml中加入下面的配置， 这样搜索页面才能访问到algolia中的索引数据:
```toml
#algolia 前端网站搜索配置
#algolia web config
algolia_search = true
algolia_appId = ""
algolia_indexName = ""
#search key
algolia_apiKey = ""
```
ArtWhite主题已经支持Algolia, 所以你可以构建的网站。

5.生成索引文件: 进入hugo项目的目录然后允许下面的命令:

中英文分词(它需要JAVA环境):
```bash
$ java -jar naah-algolia-builder-0.0.1.jar
```

英文分词:
```bash
$ ./compile
```
打开你的搜索页: http://localhost:1313/search

### 4.3 统计

我们可以开启和关闭百度统计. 在配置中填写你的id

```toml
ba_track_id  = "XXXXXXXXXXXXXXXX"
```
ba_track_id为空则是不开启百度统计

### 4.4 页面计数

页面计数我们使用的是[不蒜子](http://busuanzi.ibruce.info/)提供的服务. 如果你想开启这个功能，填写下面的配置.
```toml
page_view_conter = true
```
你也可以通过page_view_conter 来关闭它

### 4.5 文章浮动目录

文章浮动目录是我自己写的.如果你想开启他，填写下面的配置。
```toml
floatting_directory_enable=true
```
你也可以通过floatting_directory_enable来关闭它

### 4.6 侧边栏标签
如果你想开启侧边栏标签，就在配置中填写它
```toml
sidebar_tags_enable = true
```
你也可以通过sidebar_tags_enable来关闭它

### 4.7 页码按钮
如果你想开启页码，就在配置中填写它
```toml
page_enable = true
```
你也可以通过page_enable来关闭它

## 5 服务、编译和部署

### 5.1 本地服务脚本
运行下面的命令，本地服务就会启动
```bash
$ ./server
```
[`http://localhost:1313`](http://localhost:1313)

### 5.2 编译脚本

运行下面的命令，将会编译html和索引，但是只有英文索引
```bash
$ ./compile
```

运行下面的命令，将会编译html和中英文索引。(千万别删complie,因为它依赖compile脚本 )
```bash
$ java -jar naah-algolia-builder-0.0.1.jar
```

### 5.3 部署脚本

如果这是你第一次部署,先初始化git仓库.运行下面的命令
```bash
$ git init
$ git remote add <仓库短名> <git仓库链接>
```

**修改脚本**

如果你只使用英文索引,用下面的命令替换'java -jar naah-algolia-builder-0.0.1.jar'
```bash
./compile
```

修改deploy脚本,吧最后一行换掉.(如果你有多个仓库，可以把他们都写上)
```bash
git push -f <仓库短名> <本地分支名>:<远程分支名>
```

然后运行下面的命令，它将按顺序的编译html，生成索引，然后部署到远程仓库.
```bash
$ ./deploy
```

## 6 感谢
感谢[hugo-theme-cleanwhite](https://github.com/zhaohuabing/hugo-theme-cleanwhite)这个art white 的基础主题。

## 7 反馈
如果你遇到什么问题, 可以[提问题](https://github.com/naah69/hugo-theme-artwhite/issues/new)或者发现bug进行pr
