---
title: "Use Hugo + Github Pages to build a blog"
date: 2021-05-29T14:34:37+08:00
draft: false
categories:
- Web
tags:
- Hugo
- Github Pages
keywords:
- Hugo
- Github Pages
showTags: true
showPagination: true
summary: "用Hugo 和Github pages 建一個自己的部落格"
---

# 1. What is static site genertor?
最近思考很多方式來架自己的部落格, 研究的方式有

    - Medium (始終無法適應他的排版, 且文字沒有顏色orz)
    - Google Blog (一開始是用這個, 但覺得layout 很醜...)
    - WordPress
    - Django
    - Flask (更輕量化的python web framework)

考量到主機成本和建置的時間成本最終還是選擇用靜態網頁的方式來呈現。
畢竟我的部落格主要以技術筆記為主, 不太需要後端資料庫管理。

 **靜態網頁產生器(static site generator)** 是將文章預先編譯成靜態網頁, 
再將其上傳到主機, 而web users 造訪網頁時可以直接看到網頁內容。
(動態網頁需要browser 與server database query 資料再經由template 呈現)

靜態網頁產生器可分為三部分[1]

    - Content: 文章內容, 我用Markdown language 撰寫
    - Template: 部落格的樣板
    - Site-generating engine: 網頁生成器本身

網路上有許多關於靜態網頁產生器的比較(Jekyll, Hexo, Hugo)就不在此討論。
最後產生器的部分我選用Hugo, 而deploy 網頁的主機我用Github Pages

# 2. What is Hugo?
  Hugo 是用Go 開發的一套靜態網站產生器, 能夠快速佈建靜態網站，且安裝簡單，
  有許多免費的主題可以套用, 完全不懂前端語言也可以用！

# 3. Brief concept
  用一張簡單描述接下來要做的事
  ![](https://i.imgur.com/1FncZaf.png)

  首先要先在local pc 端用hugo 建立blog folder, 並下載好themes 和建立文章(i.e a post)。
  接著再Github 上建立一個github pages, 其實就是一個repo。
  接著用git remote 指令將local blog folder 與github pages 連接起來。
  最後將build 好的post push 到github上就可以看到文章囉！
  (每次都要build 整個local blog folder很麻煩, 所以我們用官網教的github action 方式
  讓Github 能夠在我們push post之後自動build 好所有的檔案)

  接下來記錄我的建置方法(感謝[2] 提供詳細解說)
# 4. Install tools
  以macOS為例(brew 是因為先安裝好Homebrew 囉)
  , 這裏需要安裝:
  - Git
    - cmd: `brew install git`
  - Hugo
    - install cmd: `brew install hugo`
    - check version: `hugo version`
  看到hugo 的版本號即為安裝成功

# 5. Create a Hugo project
  - Create a new folder for my hugo project(hugo new site [your blog name])
    - cmd: `hugo new site myblog`
  - initalize local git repo
    - cmd: `git init`

  接著`cd myblog` 進入到myblog 會看到hugo 建好的folders, 也會看到一個config.toml

  我們之後會複製themes 裡的config.toml 到這裡(覆蓋掉原先的config.toml)。
  與themes 相關的layout 設定都可以透過改動config.toml來完成。
  

# 6. Download a theme for my site
  接著到[Hugo Themes](https://themes.gohugo.io/) 上選一個喜歡的themes 用git command下載。
  我選用的是[Tranquilpeak](https://github.com/kakawait/hugo-tranquilpeak-theme)

  1. 如果你想要自己改themes 就用(copy source code 下來可以自己modify + compile)
  `git clone https://github.com/kakawait/hugo-tranquilpeak-theme.git themes/tranquilpeak`

  2. 如果沒有要自己改themes就用(建立一個link 指向此repo, 所以無法自己改檔案的喔)
  `git submodule https://github.com/kakawait/hugo-tranquilpeak-theme.git themes/tranquilpeak`

  目前我用方法2, 另外上述指令後面指定themes/tranquilpeak 是在themes裡建立名為tranquilpeak的資料夾,
  git 會將source code download 下來。

  接下來將themes/tranquilpeak/examplesites/中config.toml copy到myblog/下
  `cp themes/tranquilpeak/examplesistes/config.toml myblog/config.toml`

  接著我們可以打開myblog/config.toml 稍微改動一些blog基本的設定

  ```
  baseURL = "https://tingluotw.github.io"
  languageCode = "zh-tw"
  defaultContentLanguage = "zh-tw"
  title = "Ting's Tech Blog"
  theme = "tranquilpeak"
  disqusShortname = "tranquilpeak"
  picture = "https://i.imgur.com/TiymI9o.jpg"
  ```
  - baseURL: github pages URL
  - 語言相關的config設為"zh-tw"
  - Blog title 帶入自訂字串
  - theme 和disqusShortname 填入themes中下載的主題資料夾名稱
  - picture 是部落格大頭貼, 可以存在CDN 上, 這邊直接放link

# 7. Create a post on my site
  在myblog root folder下用hugo 指令建立一個文章,hugo 會在myblog/content下根據指令中指定路徑(post/)建立一個md file
  `hugo new post/my-first-post.md`

  md file 就可以用markdown language 來寫文章啦～學習markdown 語法又是另一回事了...
  另外實驗發現用vim post/xxx.md 建立的md file 一樣可以顯示出來(不過沒有hugo 幫你產生的post meta data在前面)
  看起來hugo 會用時間來排序所有的文章。另外注意 **所有md files 一定要放在post/下** , 至於post/下有
  多少層folders都沒關係

  打開md file 後會看到前面用---包起來的meta data(用來描述這個md file 的屬性)
  
  ```
  ---
  title: "my-first-post"
  date: 2021-05-31T18:17:42+08:00
  categories:
  - category
  - subcategory
  tags:
  - tag1
  - tag2
  keywords:
  - tech
  draft: true
  ---
  ```
  除了tile, date, draft 之外, 其他不一定需要
  - title: 文章名稱(必要)
  - date: 日期(必要)
  - categories: 為此文章分類, 會出現在部落格sidebar：分類(呈現上是link, 待研究)
  - tags: 可為文章標tags, 方便於部落格中搜尋
  - keywords: browswer 可根據keywords 搜尋到此文章
  - draft: default 為true 代表這只是一個草稿, 發佈前設為false即可

  更多的屬性可參考Tranquilepeak 作者文件中的Front-matter settings
 
# 8. Run hugo
  現在可以local 看一下成果了, 輸入下面指令(-D 代表將draft file 也編入)

  `hugo server -D`

  打開blog 到`http://localhost:1313` 就可以看到成品,
  localhost 後面的port 要根據實際run出來的結果決定, 你可以在輸入command後看到

  接著是很多blogs 教學會提到的: 將local build好的檔案放到remote github上,
  這裏可以了解一下, 但我們不會實際用到(注意不一定要-D, 當有draft post才需要-D):

  `hugo -D`
  此時會發現myblog/下多一個public folder, 沒錯, 編譯好的東西都在這, 只要將這個public folder
  push 到github上就可以看到部落格了。

  不過我們要採用一個更方便快速的方法, 用github actions, 這裏先將public folder 刪掉,
  我們後面會提到actions部分
  `rm -rf public`

# 9. Connect my local content(site) with Github
  接下來到github create 一個repo
  
  記得repo name 必須要是username.github.io (github pages 規定)。
  
  且任何選項都不要勾, 不用建立readme.md。(否則這個repo一開始就會有main branch, 會為之後git push files 衍生一些問題 )

  接著回到local 端輸入下面command:

   - create a README.md `echo "# Ting's Blog..." >> README.md`

   - add all the files in this repo `git add .`

   - set current local branch to master `git branch -M master`

   - commit this change `git commit -m "Initial commit"`

   - connect with remote github repo `git remote add origin https://github.com/tingluotw/tingluotw.github.io.git`

   - push the changes to the rmeote repo `git push origin master`

  - Note
    - 當repo沒有任何commit時無法產生任何branch, 因此git branch那行指令應該是無意義,
      當commit 後在下git branch指令就會看到master branch了
    - git remote 指令是在local 創造一個叫remote 的指標, 他會指向指定的遠端repo, 所以對local git來說,
      origin 代表遠端repo
    - git push origin master 將local master branch的commit push到remote origin branch

  此時還無法在https://tingluotw.github.io 看到完成的blog, 這是因為我們還沒設定github actions來build 文件

# 10. Update my changes automatically
  對於Deploy Hugo as a Github pages, [官網](https://gohugo.io/hosting-and-deployment/hosting-on-github/)有詳細的說明。

  1. 我們到Github repo 的頁面, 選取topbar 的Actions選單, 接著選`set up a workflow yourself`。

  2. 將官網提供的gh-pages.yml 貼到github 上, 並將有提到branch `main`的地方全改成`master`
  (因為我們remote repo 的branch name 為master), 然後commit actions

  3. commit actions 之後會看到此repo會多一個branch `gh-pages`, 裡面的文件都是之前我們在local 端
  下指令`hugo` 後看到的/public裡的發佈文件, 這代表現在我們只要將local 端myblog 文件push到repo上,
  Gighub actions 就可以自動生成/public 文件

  4. 因為現在有兩個branch, 我們需要將gh-pages 設為default branch。到Github repo的設定頁面,
  右邊選Page 選單, 可以將default branch 由main 改成gh-pages

# 11. Release my first post
  我們還沒將第一篇post 的draft 屬性改成false, 因此目前看不到任何文章

  1. 首先用`git pull` 先將remote changes 抓下來(因為我們剛剛新增了gh-pages branch)
  2. 接著`git branch -a` 會看到兩個remote branch on this repo
  3. 將我們的post 裡的draft 屬性改為false
  4. 用git add, commit, push 將post push to repo
  
  現在點開https://tingluotw.github.io 就可以看到成果了！

# 12. Enable HTML Tags
  Hugo 目前使用的markdown parser是Goldmark, 預設不能在markdown 區塊內插入html,
  不過我們為了能夠使用html `<font>` `<table>` 等標籤, 需要將其打開

  在config.toml 中加入下列設定即可(將unsafe 設為true即可)

  ```
  [markup]
    [markup.goldmark]
      [markup.goldmark.renderer]
        unsafe=true
  ```

# 13. Little tips
  Local 端編寫文章時, 可以下指令`hugo server -D &` 將指令放到background 執行, 就能夠改完檔案馬上在
  local 端用browser 輸入http://localhost:1313 預覽文章, 不用等到push 文章到remote repo 後再看

# 14. Reference:
    [1] 使用靜態網頁產生器製作網站: https://michaelchen.tech/technical-blogging/static-site-generator/
    [2] Hugo的基本安装: https://zhuanlan.zhihu.com/p/35097705://zhuanlan.zhihu.com/p/350977057 
    [3] Hugo 貼身打造個人部落格: https://ithelp.ithome.com.tw/users/20106430/ironman/3613
    [4] Hugo 的專案結構: https://carsonwah.github.io/15213187969014.html
