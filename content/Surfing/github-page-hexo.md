---
title: 搭建 Hexo 於 Github page
date: 2022-01-12 16:50:01
categories:
- 技術
---

## 前言
其實就是一時心血來潮想要搭個blog，有事沒事可以打打字、寫寫東西，以前也曾經嘗試過Notion、Hackmd等支援markdown格式的文本工具，但一直都沒有認真使用，又或者是因為一直都沒有選擇對外公開，最後都淪為記事本的用途，著實有點可惜。所以藉著這次機會搭建一個自己公開的頁面，試看看有沒有辦法起到督促自己寫下文字的作用。
<!-- more -->
## 搭建 Hexo 必要環境
### Step 1

確認 `node.js` & `npm` 已確實安裝於工作台
```
node -v
npm -v
```
查看成功安裝與否。

### Step 2

使用 `npm install` 指令安裝 [Hexo Command Line Interface](https://hexo.io/zh-tw/)
```
npm install hexo-cli -g
```
使用 `hexo cli` 提供指令初始化 blog 並安裝下載 packages & dependencies
```
hexo init blog
cd blog
npm install
```
順道安裝部屬插件 `hexo-deployer-git`  方便之後發布至 github page
```
npm install hexo-deployer-git –save
```
### Step 3
接著就可以使用 `hexo server` 指令運行hexo於本地端 (default port:4000) 預覽
```
hexo s
```

## 準備 Github Page
### Step 1

新增一個名為 `[username].github.io` 的 repositoty
![](/images/new-repo.png)

設置成功之後即可透過 `https://[username].github.io` 來瀏覽你的 github 頁面。


## 將 Hexo 發布至 Github Page
### Step 1
於本地設置 `_config.yml`，找到 `# URL` 的區塊
```
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://kermit1013.github.io/
permalink: :year/:month/:day/:title/
...
```
並將 url 的參數設定為 `https://[username].github.io`

### Step 2
接著於同個檔案底部找到 `# Deployment`
```
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: git@github.com:kermit1013/kermit1013.github.io
  branch: main
```
並將 repo 的參數設定為 ` git@github.com:[username]/[username].github.io`

以及branch 設為 `main`  (2020/10 起，所有新建的儲存庫都會以`main`為預設branch命名，不再使用`master`)

### Step 3
執行部屬之前，為了確保沒有一些奇怪的問題通常會先執行`clean`清除快取、`generate`生成靜態檔案、最後才是`deploy`
```
hexo clean & hexo g & hexo d
```

## Issues
發布階段卡在這個屎坑好一段時間，網路上不知道哪篇資源把 `:` 誤植成 `/` ，差點沒被搞瘋。
```
repo: git@github.com/[username]/[username].github.io 
```
導致部屬的時候一直出現 `Error: Spawn failed`
```
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/.../node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (events.js:376:20)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} 
```



_明天想來寫寫文化腳本。_

