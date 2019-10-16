---
title: "Flaskでカレンダーアプリを作る"
date: 2019-10-13T21:03:06+09:00
draft: 0
---

なかなか面倒くさい。

標準ライブラリの`calendar`を使う。

週，日，がリストになった2次元配列を，2回回す。

## 新規登録（create）

aリンクで，`ins/<year>-<month>-<day>`というリンク先へ遷移させる。

ルーティングで日付だけ受け渡して，postgresqlにinsertする。

## 更新と削除（update,delete）

すべての日で，日付で検索し，レコードがあれば，aリンクを作る。

リンク先で，更新と削除をする。

「もし日付にレコードがあったら」と判定するのに苦労した。  
以前，PHPでやったときは，結果の配列があるかを調べるだけで済んだ気がする。

`dataset`では，`type`で調べると，（検索結果のオブジェクトは）配列ではないらしい。  
データを取得する通常の方法のように，`results`（複数行かわからない）を取得したら，

	for result in results:

で回して，`id`をリストに追加し，このリストの要素数が0以上かで（`len(resultsList)`）），「日付があったか」どうかを判定することにした。  

もっとも，結局，1日に複数レコードの登録を許すなら，当該日付の「検索結果」を取得し，これを回して`id`を取得して，この`id`でupdate用のリンクを作る必要はある。  
つまり，検索結果を回すのは避けられない。

CRUDアプリを作るのは，かなりの時間がかかる。  
考えものかもしれない。

## 構成

作成した順

### 一覧(cal)

週と日の2次元配列を`for in`で2回回す。

insert用に，`/ins<その日の日付>`へaリンクを作る。

update用に，すべての日付でDBを検索し，検索結果を`for in`で回して，HITした場合のレコードidを取得する。  
複数レコードの場合があるので，回す必要がある。

### insコントローラー

`/ins/<mydate>`で，日付のみ一覧から受け渡してもらい，`ins.html`テンプレートへ渡す。

	return render_template('ins.html',mydate=mydate)

### ins.html(insert用form)

ins_exeへ遷移するpostメソッドのform

	<form action="/ins_exe" method="post">

引き渡された当該日付を，dateの初期値に設定しているだけ

### ins_exeコントローラー

formのデータを，`request.form['date']`などで受け取り，DBへinsertする。

	table.insert(dict(date=date,title=title,text=text))

### updコントローラー

一覧のupd用リンクを受ける。

idを引き渡されているので，DBに接続，`result['date']`などで各データを取得し，update用formへ渡す。

	return render_template('upd.html',myId=myId,date=date,title=title,text=text)

### upd.html(update用form)

upd_exeへ遷移するpostメソッドのform

	<form action="/upd_exe" method="post">

引き渡された各データを，`input`などのタグの初期値に設定する。

### upd_exeコントローラー

formのデータを，`request.form['date']`などで受け取り，updateする。

	data = dict(id=int(myId),date=date,title=title,text=text)
	table.update(data,['id'])

### delコントローラー

レコードを削除する。

`upd.html`のaリンクで，idをurl(get)で引き渡すようにしておく。

`def del():`は予約語？なのか，エラーになったので，`def myDel():`にした。

DBに接続して，削除する。

	table.delete(id=myId)

### 水平線と改行

`app.py`で回しているところで，適当なところに水平線と改行を入れて，形を整える。

	calStr.append(Markup('<br>'))#日ごとに改行
	calStr.append(Markup('<hr>'))#週ごとに水平線

### ページネーション

このままだと，「今月」しか表示されないので，前後の月に移動するようにする。

大ハマリした。  
下のように書くと，`def cal`に飛び，変数YMにtestStrの内容を入れてくれる。

	return redirect(url_for('cal', YM=testStr))#成功

公式では，以下のところに書いてある。  
`def profile`関数に飛んでいる。

	>>> @app.route('/user/<username>')
	... def profile(username): pass
	...
	>>> with app.test_request_context():
	...  print url_for('profile', username='John Doe')
	...
	/user/John%20Doe

すごい大変で，後悔し始めた・・・

以下のように，urlに「年月文字列」(例201910)を入れて，年月のデータを保持するようにした。

	@app.route('/cal/<YM>')
	def cal(YM):

来月と，前月の「年月文字列」を作成した。

	#来月
    if (myMonth == 12):
        myYearN = myYear + 1 
        myMonthN = 1 
    else:
        myYearN = myYear 
        myMonthN = myMonth + 1 
    NMonth = str(myYearN) + str(myMonthN)
    #前月
    if (myMonth == 1): 
        myYearP = myYear - 1 
        myMonthP = 12
    else:
        myYearP = myYear 
        myMonthP = myMonth - 1 
    PMonth = str(myYearP) + str(myMonthP)

これを，index.htmlに渡して，aリンクで，来月と前月へのリンクを作った。

	<a href="/cal/{{ NMonth }}">next month</a>
    <a href="/cal/{{ PMonth }}">previous month</a>

このままだと，insertやupdateすると，今月のカレンダーに戻っておかしいので，年月を保持するように修正する。

insertは，ins.htmlに渡す日付から，年月文字列を作る。

updateは，idで抽出したレコードの日付から，年月文字列を作る。