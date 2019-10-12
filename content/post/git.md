---
title: "Git備忘"
date: 2019-10-12T17:34:09+09:00
draft: 0
---
作業ディレクトリの内容を過去のものに変更できる

    git checkout id

最後は，HEADをmasterに戻す
そうしないとdetached HEADと言われる

    git checkout master

commit後に小さな変更をして，commitを修正したいとき

    git add .
    git commit –amend

エディタが開かないgit commit –amend

    git commit –amend –no-edit

メッセージだけ修正するgit commit –amend

    git commit –amend -m “fugafuga”

変更ないのにpullしても，メッセージが出るだけなので，常にpullしておけばよい

    git pull
