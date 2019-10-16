---
title: "Dataset備忘"
date: 2019-10-14T10:15:48+09:00
draft: 0
---

`dataset.py`のような名前のファイルがあると，そのファイルを探してしまうので，attribusionエラーになる。  
複数回，はまったので注意する。

公式  
https://dataset.readthedocs.io/en/latest/quickstart.html

### 検索

あいまい検索をするときだけ，`db['posts'].table`と`table`をつける(らしい)。  
`c`は，カラムの意味と思われる。

	db = dataset.connect(DB_URL)
	table = db['posts'].table
	stmt = table.select(table.c.title.like('%t%'))
	results = db.query(stmt)
	for result in results:
	    print(result['title'])
