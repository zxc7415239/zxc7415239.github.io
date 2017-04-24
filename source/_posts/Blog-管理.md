---
title: Blog 管理
date: 2017-04-22 01:10:48
categories: Hexo
tags: Hexo
---

這邊是介紹如何管理BLOG，並能在不同電腦上寫 Blog。
<!--more-->
* [新建分支，推送源碼](#新建)
* [日常 Blog 管理](#管理)
  * [發新文章](#發新文章)
  * [Blog 維護](#維護)
  * [在其他電腦上使用](#其他)
* [Reference](#Reference)

## 新建分支，推送源碼 <a href="#新建" id="新建">#</a>
為了便於在不同的電腦上寫 Blog，需要對 Blog 內容進行同步，我們使用 Github 分支(branch)來解決這個問題。
首先到 [Github](https://github.com/) 為倉庫新增分支，分支名稱為 hexo。
並到右上方 Settings 中，將 branches 選項中的 default 改成 hexo。

<img src="https://raw.githubusercontent.com/zxc7415239/MarkdownPhotos/master/photos/%E5%89%B5%E5%BB%BA%E5%88%86%E6%94%AF.png" alt="創建分支" width="50%" height="50%" />
並到本地端放置專案的資料夾中。
```
Github                     # 放置專案的資料夾
├── yourname.github.io     # 之前生成 包含 hexo 套件的倉庫
└── MarkdownPhotos         # 放置 blog 使用圖片的倉庫
```
新增一個資料夾，名稱任意，之後要放 blog 資料夾用的。
```
Github
├── yourname.github.io
├── MarkdownPhotos
└── Blog                   # 新增
```
將 Git Bash 中的路徑定位到 ``GitHub/blog`` 中。將 Github 上的倉庫拉到本地端。
```
# git bash 上面的路徑大概長這樣
You-PC@You  /e/Documents/GitHub/blog
$ git clone git@github.com:yourname/yourname.github.io.git
```
現在 blog 中多一個 yourname.github.io 資料夾。
```
Github
├── yourname.github.io
├── MarkdownPhotos
└── Blog
    └── yourname.github.io   # 新增
```
將 Git Bash 中的路徑定位到 ``Github/Blog/yourname.github.io`` 中。
此時 Git Bash 顯示當前分支為 hexo，即 default 分支。
接下來輸入指令清空 hexo 目錄。
```
# git bash 上面的路徑大概長這樣，後面 branch 變成 hexo。
You-PC@You  /e/Documents/GitHub/blog/yourname.github.io (hexo)
$ rm -rf *
$ git add .
$ git commit -m 'init'
$ git push origin hexo
```
此動作會清空hexo分支下原有透過 ``hexo d`` 部署的 html 靜態檔案。
然後在手動將 ``Github/yourname.github.io`` 的檔案複製到 ``Github/Blog/yourname.github.io`` 中。
記得不要複製到 .git 資料夾(預設是隱藏狀態)。
```
Github
├── yourname.github.io
|   ├── .git                     # .git 不要複製到
|   └── other files/floders      # 複製這些檔案
├── MarkdownPhotos
└── Blog
    └── yourname.github.io       # 丟到這個只剩下 .git 的資料夾中
        └── .git
```
複製完後 ``Github/yourname.github.io`` 這個資料夾就能刪除了。
接下來透過指令將本地端的 Blog 源碼推送到 Github 的 hexo 目錄下。
```
You-PC@You  /e/Documents/GitHub/blog/yourname.github.io (hexo) # 目前路徑
$ git add .
$ git commit -m 'init'
$ git push origin hexo
```
注意：Blog 主配置文件 _config.yml 中的 deploy 參數，部署的分支應為 master。
以後在 hexo 分支下寫 Blog，然後 ``hexo d`` 部署到 master 分支下。

## 日常 Blog 管理 <a href="#管理" id="管理">#</a>
#### 發新文章  <a href="#發新文章" id="發新文章">#</a>
執行 ``hexo new`` 命令會在 ``/source/_posts`` 下生成 post_name.md。
編輯語法為 [Markdown](http://markdown.tw/)。
```
hexo new [layout] 'post_name'   # 發表文章
hexo new 'post_name'            # 不選layout時，預設是 post
hexo n 'post_name'              # 簡寫
```
layout 模板文件在目錄scaffolds下。
post.md 預設內容如下。
```
---
title: {{ title }}
date: {{ date }}
tags:
---
```
如果想在每篇文章中添加 categories 分類信息，只需要修改 post.md 文件即可。
```
---
title: {{ title }}  #文章名稱
date: {{ date }}    #文章發布日期
categories:         #文章分類（可以為空）
tags:               #文章標籤（可以為空），多標籤格式[tag1,tag2,tag3]
---
```
不想在首頁顯示全文可使用摘要，在 post_name.md 中使用。
```
以上是摘要
<!--more-->
以下是剩下文章
```
more 以上內容即是摘要信息，顯示在首頁中，點擊 Read More 按鈕打開全文。

#### Blog 維護 <a href="#維護" id="維護">#</a>
日常發文，修改文章都在 hexo 分支下進行，平時先透過本地伺服器編輯和觀看內容。
```
hexo s
```
編輯完成後，執行命令產生靜態頁面並部署到 master 分支。
```
hexo g
hexo d
```
然後執行命令將修改過的源碼推送至 hexo 分支。
```
You-PC@You  /e/Documents/GitHub/blog/yourname.github.io (hexo) # 當前路徑
git add .
git commit -m 'XXX'
git push origin hexo
```
最後通過 ``https://yourname.github.io/`` 查看。

#### 在其他電腦上使用 <a href="#其他" id="其他">#</a>
安裝 [Node.js](https://nodejs.org/en/) 和 [Github Desktop](https://desktop.github.com/)
用 Github Desktop clone 到本地端。(能幫你弄好SSH Key)
配置好環境。千萬不要使用 ``hexo init``。
```
$ npm install -g hexo-cli
$ npm install
$ npm install hexo-deployer-git --save
```
安裝完成後就可以進行 Blog 維護了。
編輯完成後，接下來是同步的工作。
```
$ git add .               # 添加源文件
$ git commit -m ""        # git提交
$ git pull origin hexo    # 先拉原來Github分支上的源文件到本地，進行合併
$ git push origin hexo    # 比較解決前後版本衝突後，push源文件到Github的分支
```

```
$ git status # 可以檢查狀態
```
## Reference  <a href="#Reference" id="Reference">#</a>
[Hexo搭建个人博客](http://yurixu.com/categories/Hexo/)
[多设备同步hexo搭建的Github博客](http://www.jianshu.com/p/6fb0b287f950)
[Hexo博客(3)源码备份及不同电脑上的同步问题](http://masikkk.com/blog/hexo-3-source-backup-and-sync-between-diff-computer/)