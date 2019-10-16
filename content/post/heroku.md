---
title: "Heroku備忘"
date: 2019-10-13T16:21:22+09:00
draft: 0
---

【Python】PythonプログラムをHerokuにデプロイする方法 - Qiita  
https://qiita.com/1-row/items/80f89c8ada2e61f04446

公式チュートリアル  
https://devcenter.heroku.com/articles/getting-started-with-python#deploy-the-app

## flaskの場合

仮想環境を作成

	python3 -m venv myproject
	cd myproject

pipでflaskとgunicornをinstall
最低限のapp.pyを作成

	git init
	git add .
	git commit -m”1st commit”
	heroku create “app191006”（リモートの紐づけもされる）
	heroku addons:create heroku-postgresql:hobby-dev
	git push heroku master
	heroku open

アプリをコマンドで削除する場合

	heroku apps:destroy –app (app name)

datasetを`pip install`するときは，psycopg2も`pip install`する。
そうしないとエラーになる。

`heroku pg:psql`で，そのディレクトリに紐付いたアプリに紐付いたデータベースに接続できる

sql文は，大文字でなければダメらしい

	db_uri = os.environ.get('DATABASE_URL')

https://qiita.com/croquette0212/items/9b4dc5377e7d6f292671

db_uriのところの変数として，公式をコピペして`DATABASE_URL`を使っていたら，接続できず大ハマリした。

datasetを使うなら，以下のようになる。

	db = dataset.connect('db_uri')

	echo python-3.6.9 > runtime.txt
	pip freeze > requirements.txt
	web: gunicorn app:app > Procfile

app:appは，app.py(ファイル名)のappというアプリを実行する。

app.pyを実行する。

	heroku pg:psql
	¥i createTable.txt
