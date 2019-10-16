---
title: "Hugo備忘"
date: 2019-10-12T17:41:41+09:00
draft: 0
---
記事作成  

    hugo new post/[title].md

記事編集  

    open content/post

sumlime textで編集

ローカルのブラウザで確認

    hugo server --watch
    hugo server -D

コード(markdown記法)  
半角スペース4文字かタブ  
vimなら，>>でタブ入力

false または 0 にする

    draft: true

デプロイ

    ./deploy.sh
