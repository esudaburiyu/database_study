# 参考URL
[うなの日記 内部結合と外部結合](http://d.hatena.ne.jp/unageanu/20070601/1180687226)

#注意
##whereで結合するのはアウト?
whereの条件で内部結合する手法はよく使われるけど、規格的にはjoinを使うべき???

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


