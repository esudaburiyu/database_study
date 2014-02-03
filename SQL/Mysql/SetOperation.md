#MySQLで集合演算
和、差、共通、直積をやってみる。

-> 差も共通もMySQLではさぽーとされていなかった……。


#実験で使う表
~~~
mysql> select * from table1;
+------+------+-------+
| id   | big  | small |
+------+------+-------+
|    1 | AAA  | aaa   |
|    2 | BBB  | bbb   |
|    3 | CCC  | ccc   |
|    4 | DDD  | ddd   |
+------+------+-------+
4 rows in set (0.00 sec)

mysql> select * from table2;
+------+------+-------+
| id   | big  | small |
+------+------+-------+
|    1 | AAA  | aaa   |
|    2 | BBB  | bbb   |
|    3 | CCC  | ccc   |
|    5 | EEE  | eee   |
+------+------+-------+
~~~
#実験で使う表の用意
~~~
create database set_op_test;
use set_op_test

create table table1 (id int, big varchar(256), small varchar(256));
insert into table1 (id, big, small) values (1, 'AAA', 'aaa');
insert into table1 (id, big, small) values (2, 'BBB', 'bbb');
insert into table1 (id, big, small) values (3, 'CCC', 'ccc');
insert into table1 (id, big, small) values (4, 'DDD', 'ddd');

create table table2 (id int, big varchar(256), small varchar(256));
insert into table2 (id, big, small) values (1, 'AAA', 'aaa');
insert into table2 (id, big, small) values (2, 'BBB', 'bbb');
insert into table2 (id, big, small) values (3, 'CCC', 'ccc');
insert into table2 (id, big, small) values (5, 'EEE', 'eee');
~~~

#和 UNION
コマンド
~~~
select * from table1 union select * from table2;
~~~
結果
~~~
+------+------+-------+
| id   | big  | small |
+------+------+-------+
|    1 | AAA  | aaa   |
|    2 | BBB  | bbb   |
|    3 | CCC  | ccc   |
|    4 | DDD  | ddd   |
|    5 | EEE  | eee   |
+------+------+-------+
~~~

#共通 INTERSECT
######MySQLではINTERSECTがサポートされてないらしい!!!

#差
######差もMySQLにはない
ないので、副問い合わせを用いて実現する

#直積 CROSS JOIN
コマンド
~~~
select * from table1 CROSS JOIN table2
~~~

~~~
+------+------+-------+------+------+-------+
| id   | big  | small | id   | big  | small |
+------+------+-------+------+------+-------+
|    1 | AAA  | aaa   |    1 | AAA  | aaa   |
|    2 | BBB  | bbb   |    1 | AAA  | aaa   |
|    3 | CCC  | ccc   |    1 | AAA  | aaa   |
|    4 | DDD  | ddd   |    1 | AAA  | aaa   |
|    1 | AAA  | aaa   |    2 | BBB  | bbb   |
|    2 | BBB  | bbb   |    2 | BBB  | bbb   |
|    3 | CCC  | ccc   |    2 | BBB  | bbb   |
|    4 | DDD  | ddd   |    2 | BBB  | bbb   |
|    1 | AAA  | aaa   |    3 | CCC  | ccc   |
|    2 | BBB  | bbb   |    3 | CCC  | ccc   |
|    3 | CCC  | ccc   |    3 | CCC  | ccc   |
|    4 | DDD  | ddd   |    3 | CCC  | ccc   |
|    1 | AAA  | aaa   |    5 | EEE  | eee   |
|    2 | BBB  | bbb   |    5 | EEE  | eee   |
|    3 | CCC  | ccc   |    5 | EEE  | eee   |
|    4 | DDD  | ddd   |    5 | EEE  | eee   |
+------+------+-------+------+------+-------+
16 rows in set (0.00 sec)
~~~
