# 参考URL
[うなの日記 内部結合と外部結合](http://d.hatena.ne.jp/unageanu/20070601/1180687226)

#注意
##whereで結合するのはアウト?
whereの条件で内部結合する手法はよく使われるけど、規格的にはjoinを使うべき???

#疑問点
内部結合、自然結合、外部結合。
自然結合は、内部結合から余計なフィールドを削ったもの、と思っていたが、ちょっと違うみたい。。。

右か左か、内部か外部か、自然かそうでないか……。
ちゃんとした違いがよくわからない。

内部か外部か、はNULLを入れるかどうか、くらいの違いだと思っていた。しかしだとすると、左内部結合や右内部結合なんてものがあるはずない。内部結合は、片方にだけあるやつは消すはずだから。

#わかったこと
結局、自然かどうかは、重複を削るか削らないか。

自然右外部結合、自然左外部結合、自然完全外部結合はありうる。

自然内部結合もありうる。

外部結合について
自然かそうでないか、右、左、完全のいずれか
だから、外部結合には理論上六種類ある。

内部結合について
自然かそうでないか。
だから内部結合は二種類のはず。右とか左とか完全とかはないはず。
##MySQLでは完全外部結合はサポートしていないよう
[SQLでの完全外部結合](http://mistymagich.wordpress.com/2011/01/06/mysql%E3%81%A7%E3%81%AE%E5%AE%8C%E5%85%A8%E5%A4%96%E9%83%A8%E7%B5%90%E5%90%88/)


#実験に用いる表
~~~
mysql> select * from big;
+------+-------+
| id   | text  |
+------+-------+
|    1 | AAAAA |
|    2 | BBBBB |
|    3 | CCCCC |
|    4 | DDDDD |
+------+-------+
4 rows in set (0.00 sec)

mysql> select * from small;
+------+-------+
| id   | text  |
+------+-------+
|    1 | aaaaa |
|    2 | bbbbb |
|    3 | ccccc |
|    5 | eeeee |
+------+-------+
4 rows in set (0.00 sec)
~~~
#実験するための表を用意
~~~
create database jointest;
create table big(id int, text varchar(255));
create table small(id int, text varchar(255));
insert into big (id,text) values (1,'AAAAA');
insert big (id, text) values (2, BBBBB);
insert big (text, id) values ('CCCCC', 3);
insert into big (id, text) values (4, 'DDDDD')

insert into small (id, text) values (1, 'aaaaa');
insert into small (id, text) values (2, 'bbbbb');
insert into small (id, text) values (3, 'ccccc');
insert into small (id, text) values (5, 'eeeee');
~~~
######intoを忘れたのに、ちゃんと動いている???
######フィールド指定と値指定の対応がとれていれば、順番は入れ替えても大丈夫なのは、思った通り
######insertのループ文みたいなのはないのか?

#実際の実験
##内部結合 A join B
###コマンド
~~~
select *
from big join small
on big.id = small.id;
~~~
###結果
~~~
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | AAAAA |    1 | aaaaa |
|    2 | BBBBB |    2 | bbbbb |
|    3 | CCCCC |    3 | ccccc |
+------+-------+------+-------+
~~~
######結果では、idの値が削除されずに出てきた

##左結合 A left join B
###コマンド
~~~
select *
from big left join small
on big.id = small.id;
~~~
###実行結果
~~~
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | AAAAA |    1 | aaaaa |
|    2 | BBBBB |    2 | bbbbb |
|    3 | CCCCC |    3 | ccccc |
|    4 | DDDDD | NULL | NULL  |
+------+-------+------+-------+
~~~

##右結合 A right join B
###コマンド
~~~
select *
from big right join small
on big.id = small.id;
~~~
###実行結果
~~~
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | AAAAA |    1 | aaaaa |
|    2 | BBBBB |    2 | bbbbb |
|    3 | CCCCC |    3 | ccccc |
| NULL | NULL  |    5 | eeeee |
+------+-------+------+-------+
~~~

##右結合と左結合って、項を入れ替えれば同じ?
つまり、 A right join B とB left join A って同じ意味かどうか

###実験
~~~
mysql> select * from big right join small on big.id = small.id;
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | AAAAA |    1 | aaaaa |
|    2 | BBBBB |    2 | bbbbb |
|    3 | CCCCC |    3 | ccccc |
| NULL | NULL  |    5 | eeeee |
+------+-------+------+-------+
4 rows in set (0.00 sec)

mysql> select * from small left join big on big.id = small.id;
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | aaaaa |    1 | AAAAA |
|    2 | bbbbb |    2 | BBBBB |
|    3 | ccccc |    3 | CCCCC |
|    5 | eeeee | NULL | NULL  |
+------+-------+------+-------+
4 rows in set (0.00 sec)
~~~
###結果
実際はちょっと違う。具体的には帰ってきた時のフィールドの順番が違う。
しかし、データベースではフィールドの順番の違いは本質的ではないはず。

#自然結合
自然結合には、いろんなやりかたがある。
MySQLではどうやってできるか?

## INNER JOINを試してみる
結局、INNER JOIN の結果から、列を結合する。
この手法だと、テーブルのフィールドが大きくなるほど、面倒になるはず。

~~~
select big.id, big.text, small.text
from big inner join small
on big.id = small.id;
~~~
###結果
~~~
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | AAAAA | aaaaa |
|    2 | BBBBB | bbbbb |
|    3 | CCCCC | ccccc |
+------+-------+-------+
~~~

##よくあるやつ
~~~
 select big.id, big.text, small.text
 from big, small
 where big.id = small.id;
~~~
実行結果
~~~
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | AAAAA | aaaaa |
|    2 | BBBBB | bbbbb |
|    3 | CCCCC | ccccc |
+------+-------+-------+
~~~

## INNER JOIN , USINGを試してみる
以下２つはエラー
~~~
select * from big inner join small using id;
~~~
~~~
select * from big natural inner join small;
~~~

######これじゃ、MySQLには自然結合を簡潔にやる方法ないじゃん!


## natural join
~~~
select * from big natural join small;
~~~
~~~
+------+-------+
| id   | text  |
+------+-------+
|    1 | AAAAA |
|    2 | BBBBB |
|    3 | CCCCC |
+------+-------+
~~~

######あれ? うまくいかない……。

## join , using
~~~
 SELECT * FROM big JOIN small using (id);
~~~
~~~
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | AAAAA | aaaaa |
|    2 | BBBBB | bbbbb |
|    3 | CCCCC | ccccc |
+------+-------+-------+
~~~
######あれ? うまくいった……。

#inner join, left outer join, right outer join
~~~
mysql> select * from big inner join small using (id);
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | AAAAA | aaaaa |
|    2 | BBBBB | bbbbb |
|    3 | CCCCC | ccccc |
+------+-------+-------+
3 rows in set (0.00 sec)

mysql> select * from big right outer join small on big.id = small.id;
+------+-------+------+-------+
| id   | text  | id   | text  |
+------+-------+------+-------+
|    1 | AAAAA |    1 | aaaaa |
|    2 | BBBBB |    2 | bbbbb |
|    3 | CCCCC |    3 | ccccc |
| NULL | NULL  |    5 | eeeee |
+------+-------+------+-------+
4 rows in set (0.00 sec)

mysql> select * from big left outer join small using (id);
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | AAAAA | aaaaa |
|    2 | BBBBB | bbbbb |
|    3 | CCCCC | ccccc |
|    4 | DDDDD | NULL  |
+------+-------+-------+
4 rows in set (0.00 sec)

mysql> select * from big right outer join small using (id);
+------+-------+-------+
| id   | text  | text  |
+------+-------+-------+
|    1 | aaaaa | AAAAA |
|    2 | bbbbb | BBBBB |
|    3 | ccccc | CCCCC |
|    5 | eeeee | NULL  |
+------+-------+-------+
4 rows in set (0.00 sec)
~~~
これを見ると、
~~~
mysql> select * from big inner join small using (id);
~~~
が、自分が自然結合に期待するような挙動をしている。

usingを使うことで、自然結合みたいなことができている?

outer join のほうも、usingをつかうことで、自然結合っぽくなっている?
