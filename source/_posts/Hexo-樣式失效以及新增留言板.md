---
title: 解決 Hexo 部屬到Github 畫面空白 & 開啟disqus留言板
tags: 
- hexo
- disqus
- Travis CI
- Github
abbrlink: 7251
date: 2021-02-04 22:10:35
---


 ## 首次將Hexo部屬到Github上 

先依照官方的做法，將hexo部屬到github，並完成 travis CI 自動將檔案部屬到 gh-page 分支
https://hexo.io/zh-cn/docs/github-pages.html

 然後就出問題了。
 連結到gh-page的網址後會看到一片空白。

 這時候可在github repo中 切到Travis CI生成的gh-page分支，裡面應該會找不到 style.css
 推測可能是theme的問題，切回master分支檢查，確認theme資料夾確實沒有被上傳。


 ### 推測可能的錯誤

 因為theme是clone下來的，所以其實theme中有.git檔案，這會造成推送到github錯誤



 ### 解決方法
用指令刪除 theme 資料夾內的 .git 但這樣整個 hexo專案資料夾內的 git紀錄都會出錯，要操作git時都會回傳 fatal: Not a git repository (or any of the parent directories) 錯誤。

我的解決方式是把整個hexo專案的git清除重下指令 git init，再重開一個新的github repo，畢竟第一次推送，也沒有需要版控的地方。

覺得重開泰麻煩的畫，可以用fork的方式，直接把theme作者的 github 專案 fork到自己的repo，這個部分就不贅述。


### 開啟留言板

我在主題作者的demo內看到有留言版功能，但下載回來後卻沒看到相關選項，查了資料都說可以新增config.yml 中的disqus_shortname 名稱，但下載回來的yml沒有這個選項阿！

又因為theme的script有撰寫disqus的語法，所以試著直接加入這行


     disqus_shortname: <disqus website Name>


可以使用留言板了... 以上