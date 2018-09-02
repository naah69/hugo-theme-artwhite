# Art White Theme for Hugo

# [中文文档](https://github.com/naah69/hugo-theme-artwhite/blob/master/README_cn.md)

Art White is a blog theme for Hugo. Here is a live [demo site](http://www.naah69.com) using this theme.

It is based on [Clean WhiteTheme](https://github.com/zhaohuabing/hugo-theme-cleanwhite).

## 1 New Function
1. Changyan Comments
2. English Participles With Algolia(it need node.js environment)
3. Chinese Participles With Algolia(it need java environment)
4. Busuanzi Page View Count
5. Article Floatting Directory
6. Optional Sidebar Tag
7. Page Button
8. Server、Compile And Deploy Script

## 2 Screenshots

### 2.1 Home

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/fullscreenshot.png)

### 2.2 Post

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/post.png)

### 2.3 Search

![screenshot](https://raw.githubusercontent.com/naah69/hugo-theme-artwhite/master/images/search.png)

## 3 Quick Start
1.Go to the directory where you have your Hugo site and run:

```
$ mkdir themes
$ cd themes
$ git clone https://github.com/naah69/hugo-theme-artwhite.git
```

 If your site is already a git project, you may want to choose to add the ArtWhite theme as a submodule to avoid messing up your existing git repository.

```
$ mkdir themes
$ git submodule add https://github.com/naah69/hugo-theme-artwhite.git themes/hugo-theme-artwhite
```

2.copy all files in themes/hugo-theme-artwhite/requiredFile and cover to your project.

3.Run Hugo Build-in Server Locally
```
$ ./server
```
Now enter [`localhost:1313`](http://localhost:1313) in the address bar of your browser.


## 4 Configuration
First, let's take a look at the [config.toml](https://github.com/naah69/hugo-theme-artwhite/blob/master/requiredFile/config.toml). It will be useful to learn how to customize your site. Feel free to play around with the settings.

### 4.1 Changyan Comments
The optional comments system is powered by [Changyan](https://changyan.kuaizhan.com/). If you want to enable comments, create an account in Changyan and write down your config.

```toml
#畅言评论配置
#changyan comment config
changyan_enable = true
changyan_appid = ""
changyan_conf = ""
```
You can disable the comments system by changyan_enable.

### 4.2 Site Search with Algolia
1.Follow this [tutorial](https://forestry.io/blog/search-with-algolia-in-hugo/#3-create-your-index-in-algolia) to create your index in Algolia. The index is just the storage of the indexing data of your site in the the cloud . The search page of ArtWhite theme will utilize this indexing data to do the search.

2.Go to the directory where you have your Hugo site and run the following commands,it need **node.js** environment:
```bash
$ npm install hugo-algolia -g
```

3.Next, create new file named config.yaml, and add the following:
```yaml
---
baseurl: "your baseurl"
DefaultContentLanguage: "zh-cn"
hasCJKLanguage: true
languageCode: "zh-cn"
title: "your site title"
theme: "hugo-theme-artwhite"
metaDataFormat: "yaml"
algolia:
  index: "your algolia index"
  key: "your algolia admin key"
  appID: "your algolia appID"
---
```

4.Add the following variables to your hugo site config ( config.toml) so the search page can get access to algolia index data in the cloud:
```toml
#algolia 前端网站搜索配置
#algolia web config
algolia_search = true
algolia_appId = ""
algolia_indexName = ""
#search key
algolia_apiKey = ""
```
Algolia index output format has already been supported by the ArtWhite theme, so you can just build your site.

5.Generate index file: Go to the directory where you have your Hugo site and run the following commands:

Chinese And English Participles(it dependen compile a and need JAVA environment):
```bash
$ java -jar naah-algolia-builder-0.0.1.jar
```

English Participles:
```bash
$ ./compile
```
Open search page in your browser: http://localhost:1313/search

### 4.3 Analytics

You can optionally enable Baidu Analytics. Type your tracking code in the

```toml
ba_track_id  = "XXXXXXXXXXXXXXXX"
```
Leave the 'ba_track_id ' key empty to disable it.

### 4.4 Page View Count

The optional page view count is powered by [Busuanzi](http://busuanzi.ibruce.info/). If you want to enable page view count, write down your config.
```toml
page_view_conter = true
```
You can disable the page view count by page_view_conter.

### 4.5 Article Floatting Directory

The optional article floatting directory is power by me.If you want to enable it, write down your config.
```toml
floatting_directory_enable=true
```
You can disable the article floatting directory by floatting_directory_enable.

### 4.6 Sidebar Tag
If you want to enable it, write down your config.
```toml
sidebar_tags_enable = true
```
You can disable the sidebar tag by sidebar_tags_enable.

### 4.7 Page Button
If you want to enable it, write down your config.
```toml
page_enable = true
```
You can disable the page button by page_enable.

## 5 Server、Compile And Deploy

### 5.1 server script
Run the following commond.it will be started.
```bash
$ ./server
```

### 5.2 compile script

Run the following commond.it will be compile html and index,but it will generate English Index Only.
```bash
$ ./compile
```

Run the following commond.it will compile html and generate Chinese and English index.(must not delete complie,because it dependen compile script )
```bash
$ java -jar naah-algolia-builder-0.0.1.jar
```

### 5.3 deploy script

if this is your first deploy,please init your git repository.Run the following commond.
```bash
$ git init
$ git remote add <short name> <remote git url>
```

**modify the deploy script**

if you use English index only,the following command replace 'java -jar naah-algolia-builder-0.0.1.jar'
```bash
./compile
```


replace the last commond in the deploy script.(if you have multi repository,you can write them)
```bash
git push -f <short name> <local branch name>:<remote branch name>
```

Next.Run the following commond.it will compile html,generate Chinese and English Index and deploy to remote git repository one by one.
```bash
$ ./deploy
```

## 6 Thank
Thanks for the great jobs of [hugo-theme-artwhite](https://github.com/zhaohuabing/hugo-theme-artwhite)  which is the  upstream project ArtWhite Hugo theme is based on.

## 7 Feedback
If you find any problems, please feel free to [raise an issue](https://github.com/naah69/hugo-theme-artwhite/issues/new) or create a pull request to fix it.
