---
title: Hexo + Github 簡單使用教學
date: 2017-04-21 21:03:41
categories: Hexo
tags: Hexo
---

這邊主要就紀錄我使用 Hexo 部署到 Github 上的流程。
<!--more-->
* [前言](#前言)
* [建置環境](#建置環境)
* [建立流程](#建立流程)
  * [安裝 Node.js](#安裝Node)
  * [安裝 Github Desktop](#安裝Github)
  * [Github Desktop 使用](#Github使用)
  * [安裝 Hexo](#安裝Hexo)
  * [部署 Hexo 到 Github](#部署Hexo)
* [自訂義部分](#自訂義部分)
  * [程式碼區塊](#程式碼區塊)
  * [字體](#字體)
  * [Newer Older 相反](#相反)
* [常用 Hexo 指令](#常用指令)
* [Reference](#Reference)

## 前言 <a href="#前言" id="前言">#</a>
因為第一次使用 Github，Node.js 和 Hexo，所以只有會一點基礎東西。
主要是幫助從沒接觸過的人能快速上手。

## 建置環境 <a href="#建置環境" id="建置環境">#</a>
OS: Windows 8.1
[Hexo](https://hexo.io/zh-tw/): 3.3.1
[Node.js](https://nodejs.org/en/): 6.10.2 LTS
[Github Desktop](https://desktop.github.com/)

## 建立流程 <a href="#建立流程" id="建立流程">#</a>
#### 安裝 Node.js <a href="#安裝Node" id="安裝Node">#</a>
去 [Node.js](https://nodejs.org/en/) 官網下載 windows x64 版本直接安裝即可。
```
$ node -v   # 可測試是否安裝成功
>> v6.10.2
```
#### 安裝 Github Desktop <a href="#安裝Github" id="安裝Github">#</a>
去 [Github Desktop](https://desktop.github.com/) 官網下載直接安裝即可。

#### Github Desktop 使用 <a href="#Github使用" id="Github使用">#</a>
去 [Github](https://github.com/) 官網登入
新增一個倉庫(Repositories)，倉庫名稱為 ``yourname.github.io`` [yourname是你的帳號]。

<img src="https://raw.githubusercontent.com/zxc7415239/MarkdownPhotos/master/photos/%E6%96%B0%E5%A2%9E%E5%80%89%E5%BA%AB.png" alt="創建倉庫" width="50%" height="50%" />
開啟剛安裝好的 Github Desktop ，並將剛創好的倉庫存到本地端。

<img src="https://raw.githubusercontent.com/zxc7415239/MarkdownPhotos/master/photos/%E9%96%8B%E5%95%9Fgui.png" alt="存下倉庫" width="50%" height="50%" />
然後右鍵剛拉下來的倉庫，選取 Open in Git Shell 打開 Git bash(option可選)，執行指令將 Github 上的倉庫拉到本地端。
```
$ git pull origin master
```
Github Desktop 右上的 Sync 按鈕具有 pull/push 功能，不想打指令可以多嘗試。
使用 Github Desktop 的好處是不必像其他教學一樣，不需要配置 SSH Key 和 設定 origin 位置路徑，省了兩個步驟。

#### 安裝 Hexo  <a href="#安裝Hexo" id="安裝Hexo">#</a>
安裝好 Node.js 後，就能使用 npm 安裝 hexo。
```
$ npm install -g hexo-cli
```
輸入以下指令可查看版本。
```
$ hexo version
```
接下來，依序輸入以下指令，初始化我們的 Blog。
```
# git bash 上面的路徑大概長這樣
You-PC@You  /e/Documents/GitHub/yourname.github.io (master)
$ hexo init		# 初始化 blog
# 輸入上面那個指令後 hexo 會產生新的 .git蓋掉舊的 .git
$ git init              # 所以就重新產生一個 .git
$ npm install		# 安裝相關套件
$ hexo g		# 產生靜態頁面
$ hexo s		# 啟動本地伺服器
```
網址列輸入 ``http://localhost:4000`` ，就能觀看 Blog 了。
預設會有一個 Hello World 貼文。

#### 部署 Hexo 到 Github  <a href="#部署Hexo" id="部署Hexo">#</a>
到我們本地 yourname.github.io 的資料夾中能找到 _config.yml 文件。
這是 Hexo 的全域配置文件。詳細參數設定可到 [Hexo搭建个人博客（二）](http://yurixu.com/blog/2016/01/04/Hexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%BA%8C%EF%BC%89/) 查看
這邊拉到最底部修改一下 deploy 參數。
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:yourname/yourname.github.io.git
  branch: master
```
這邊是 YAML 語法，冒號後面記得空一格，照上面的設定輸入，倉庫的 SSH 地址如下圖可獲得。

<img src="https://raw.githubusercontent.com/zxc7415239/MarkdownPhotos/master/photos/SSH.png" alt="SSH地址鏈接" width="50%" height="50%" />
執行以下命令安裝 hexo-deployer-git，沒安裝套件前輸入 hexo d 會出現 Error。
```
$ npm install hexo-deployer-git --save
```
產生靜態頁面後，部署到 Github。
```
$ hexo d -g     # 等同輸入 hexo g 和 hexo d 指令
```
再來就可以上 ``https://yourname.github.io/`` 查看 Blog 了。


## 自訂義部分 <a href="#自訂義部分" id="自訂義部分">#</a>
這邊是我有用到客製化的一些紀錄
如果動過這區的東西，要砍掉舊的靜態檔案，才會換成修改過的靜態檔案。
```
$ hexo clean
```
清除快取檔案 ``(db.json)`` 和已產生的靜態檔案 ``(public)``。
#### 程式碼區塊 <a href="#程式碼區塊" id="程式碼區塊">#</a>

找到文件 ``/themes/landscape/source/css/_partial/highlight.styl`` 中17行
```
margin: 0 article-padding * -1
```
換成
```
margin: auto
```
這樣程式碼區塊就會與兩側保持一定間距。
在第22行添加
```
border-radius：8px
```
使程式碼區塊呈現圓角效果。

#### 字體 <a href="#字體" id="字體">#</a>
找到文件 ``/themes/landscape/source/css/_variables.styl`` 修改
```
font-sans = Microsoft JhengHei, "Helvetica Neue", Helvetica, Arial, sans-serif
font-size = 16px
```
#### Newer Older 相反 <a href="#相反" id="相反">#</a>
突然發現下方 Newer Older 居然連結相反了。
到 ``/themes/landscape/layout/_partial/post/nav.ejs`` 修改。
把 prev 跟 next 對調一下。
```
<% if (post.next || post.prev){ %>
<nav id="article-nav">
  <% if (post.next){ %>
    <a href="<%- url_for(post.next.path) %>" id="article-nav-newer" class="article-nav-link-wrap">
      <strong class="article-nav-caption"><%= __('newer') %></strong>
      <div class="article-nav-title">
        <% if (post.next.title){ %>
          <%= post.next.title %>
        <% } else { %>
          (no title)
        <% } %>
      </div>
    </a>
  <% } %>
  <% if (post.prev){ %>
    <a href="<%- url_for(post.prev.path) %>" id="article-nav-older" class="article-nav-link-wrap">
      <strong class="article-nav-caption"><%= __('older') %></strong>
      <div class="article-nav-title"><%= post.prev.title %></div>
    </a>
  <% } %>
</nav>
<% } %>
```
## 常用 Hexo 指令 <a href="#常用指令" id="常用指令">#</a>
發文
```
$ hexo new "postName" 		# 產生新的文章
$ hexo new page "pageName"	# 產生新的頁面
```
Hexo提供了常用命令的簡寫
```
$ hexo n == hexo new   		# 產生新的 post/page/draft
$ hexo g == hexo generate  	# 產生靜態頁面
$ hexo s == hexo server		# 啟動本地瀏覽
$ hexo d == hexo deploy		# 部署文件至 Github 上
```
指令組合
```
$ hexo d -g	# 產生靜態文件後，部署 blog
$ hexo s -g	# 產生靜態文件後，預覽 blog
```
## Reference  <a href="#Reference" id="Reference">#</a>
[Hexo + Github 零基礎建立過程](https://mousyball.github.io/2017/01/01/Hexo-Github-Build/#an-zhuang-github-desktop)
[Hexo搭建个人博客](http://yurixu.com/categories/Hexo/)
[添加回滚到顶部按钮](http://charsdavy.github.io/2016/06/24/hexo-extern-scroll-top/)
[Git Book](https://git-scm.com/book/zh-tw/v1)