---
title: "Postgresql備忘"
date: 2019-10-13T20:40:43+09:00
draft: 0
---

psql -l　データベース一覧  
createdb データベース名　データベース作成  
dropdb データベース名　データベース削除  
psql データベース名　データベースに接続  

help ヘルプが出る  
\? よく使うコマンド  
\l データベース一覧  
\q 終了（quit）  

テーブル作成

	create table テーブル名 (id SERIAL, title varchar(255), body text);

`SERIAL`は，自動採番

\dt すべてのテーブルのスキーマ（data table?）  
\d テーブル名 特定のテーブルのスキーマ  

テーブル名の変更

	alter table テーブル名 rename to 新テーブル名

テーブル削除

	drop table テーブル名

外部ファイルのコマンドを実行  
\i ファイル名　インタラクティブではないのに。

レコード挿入

	insert into テーブル名 (title,body) values(‘hoge’,‘foo’);

複数レコードを挿入

	insert into テーブル名 (title,body) values
	(‘hoge1’,‘foo1’),
	(‘hoge2’,‘foo2’),
	(‘hoge3’,‘foo3’);
