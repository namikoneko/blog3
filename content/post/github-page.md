---
title: "Hugoで作ったサイトをGithubPageで公開する"
date: 2019-10-12T18:35:56+09:00
draft: 0
---
初めは，publicディレクトリだけgitで管理してgit pushしてGithubPageに公開していました  
しかし，「本体」は，contentディレクトリにあるmdファイルなので，publicディレクトリの静的ファイルだけgithubにあげると，他のPCからブログを更新できません  

そこで，公式の情報も見て，hugo new siteで作成したディレクトリもgithubにあげることにしました

公式  
https://gohugo.io/hosting-and-deployment/hosting-on-github/

参考サイト  
https://qiita.com/h6m3_u/items/5893a61091d258936716

公式を途中までやり，deploy.shの後半は，参考サイトを参考にしました
