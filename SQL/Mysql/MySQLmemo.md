MySQLのコマンドチートシート

#MySQLの起動、終了
##MySQLの起動
~~~
mysql -u root -p
~~~
##MySQLの終了
~~~
exit;
~~~

#データベースに対する操作
##データベースの作成
~~~
create database "データベース名";
~~~
##データベースの一覧
~~~
show databases;
~~~
##データベースの削除
~~~
drop database "データベース名";
~~~
##データベースの切り替え
~~~
use "データベース名";
~~~

###### MongoDBと結構似てる。

##データベースに関する権限をユーザーに与える
~~~
grant all on "データベース名".* to "ユーザー名"@localhost identified by "パスワード";
~~~
ユーザーは
~~~
mysql -u "ユーザー名" -p "データベース名"
~~~
でログインして作業できるようになる。

#テーブルに関する操作
##テーブルを作成する
~~~
create table "テーブル名" ("フィールド名1" "フィールドの型1",  "フィールド名2" "フィールドの型2" ...);
~~~

##テーブルの構造を見る
~~~
desc "テーブル名";
~~~

##テーブルを削除する
~~~
drop table "テーブル名";
~~~

#レコードを操作する

##レコードを挿入する insert文
~~~
insert into "テーブル名 " ("フィールドのリスト") values ("値のリスト);
~~~

フィールドのリストは、省略しても大丈夫

~~~
insert into "テーブルの名前" values 値のリスト
~~~
フィールドに左から対応する値がセットされる。

##レコードの一覧を見るには select文
~~~
select * from "テーブル名";
~~~

###ちょっと便利な記法
select * from "テーブル名" \G
このとき、セミコロンはいらない
出力形式が整形されてでてくる。

###where
~~~
select * from "テーブル" where "条件"
~~~
<>, !=,

~~~
select * from "テーブル" where "フィールド" like "条件"
~~~
条件部では、
~~~
'%.com'
~~~
%は任意の文字列、_は任意の一文字を表す。

### 並び替え order by
~~~
select * from "テーブル名" order by "フィールド名";
select * from "テーブル名" order by "フィールド名" desc;
~~~

### 制限 limit
~~~
select * from score order by score limit 2;
select * from score order by score limit 2, 2;
~~~

##レコードを更新する update文

##レコードを削除する delete文
~~~
delete from "テーブル名" where "条件";
~~~

#表を結合する

##直積 A cross join B
~~~
select *
from A cross join B;
~~~

##内部結合 A join B
~~~
select *
from A join B
on A.id = B.id;
~~~

##左結合 A left join B
~~~
select *
from A left join B
on A.id = B.id;
~~~

##右結合 A right join B
~~~
select *
from A right join B
on A.id = b.id
~~~

#集合演算
##

#おもしろ
##乱数を取得
~~~
select rand();
~~~

##ランダムにフィールドを一つ
~~~
select * from "テーブル" order by rand() limit 1;
~~~

#文字列周り
select length(テキスト) from
select



