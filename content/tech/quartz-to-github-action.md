---
created: 2024-01-25T03:56
updated: 2024-01-27T03:24
title: Quartz 整合 Github Action
---
 2024折騰自己的開始，為了避免下次回來又得重新回想整個發佈的流程，第一篇就用來充作README.md吧。
# Initialization
Fork quartz 到自己的 Github 下面。

> Use this template
> ![[Pasted image 20240125125921.png]]


```bash
git clone [your repo fork from jackyzha0/quartz]
cd quartz
npm i
npx quartz create 
```

> Symlink content folder to your obsidian folder. 
>![[Pasted image 20240125040103.png]]

# Index Page
以 [[index]] 作為 quartz 的進入點(首頁)，必須先創建一個`index.md` 於 content資料夾。

```bash
├── .github
├── content
│   ├── ...
│   ├── index.md   <= 進入點
├── node_modules
├── package.json
├── package-lock.json
├── quartz.config.ts
└── .gitignore
```

# Private Pages
*因為在Symlink時我是選擇連接到整個Personal* vault ，所以如果不希望整個vault都公諸於世的話，記得設定另外設定 ` quartz.config.ts` 、 `.gitignore` 這兩檔案。
## quartz.config.ts

> Using "00 - Private" in my case 
>![[Pasted image 20240125141808.png]]


> [!TIPS] Common examples include:
> - `some/folder`: exclude the entire of `some/folder`
>- `*.md`: exclude all files with a `.md` extension
>- `!*.md` exclude all files that _don’t_ have a `.md` extension
>- `**/private`: exclude any files or folders named `private` at any level of nesting
## .gitignore

> Using "content/00 - Private" in my case
> ![[Pasted image 20240125142114.png]]


# Hosting
這邊就是直接參考官方doc做法照抄過來基本上沒有其他問題。
## deploy.yml
新增 `deploy.yml` 於 `.github/workflows/` 下。
```bash
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v3
        with:
          node-version: 18.14
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

## GitHub Actions

隨後前往該 repo 的 `Settings` -> `Pages` -> `Build and deployment`

> 來源選擇 `GitHub Actions`
>![[Pasted image 20240125143240.png]]

## Commit 
提交後 quartz 將透過 github action 自動部署於 `<github-username>.github.io/<repository-name>`。

> 使用`npx quartz sync` 提交改動並推到branch `v4`  
>![[Pasted image 20240125143918.png]]