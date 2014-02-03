#目標

- 成績表の中から、平均より点数の高い生徒を求める例を通して、単純副問い合わせを修得する。
- 成績表の中から、クラスの平均より点数の高い生徒を求める例を通して、相関副問い合わせを修得する。

#実験に用いる表
~~~
 create database subquery_test;
 use subquery_test;
 create table score (class int, name text, score int);
insert into score (class, name, score) values (1, "Taro", 100);
insert into score (class, name, score) values (1, "Jiro", 80);
insert into score (class, name, score) values (1, "Sabu", 60);
insert into score (class, name, score) values (2, "Siro", 40);
insert into score (class, name, score) values (2, "Goro", 20);
insert into score (class, name, score) values (2, "Roku", 0);
~~~

#まず、単純に点数の平均を求める
~~~
select avg(score) from score;
~~~
~~~
+------------+
| avg(score) |
+------------+
|    50.0000 |
+------------+
~~~

#次に、点数が平均より高い生徒の名前と点数を表示する
~~~
select name, score from score where score > (select avg(score) from score);
~~~
~~~
+------+-------+
| name | score |
+------+-------+
| Taro |   100 |
| Jiro |    80 |
| Sabu |    60 |
+------+-------+
~~~
######できたけど、テーブルの名前と、フィールドの名前でscoreを二重に使っているから、ちょっとわかりづらいな。。。

#相関副問い合わせに入る前に、クラスごとの平均を出してみる
~~~
select class, avg (score) from score group by class;
~~~
~~~
+-------+-------------+
| class | avg (score) |
+-------+-------------+
|     1 |     80.0000 |
|     2 |     20.0000 |
+-------+-------------+
~~~

#本題の、クラス平均より点数の高い生徒を出してみる
~~~
select name, score
from score s1 where score >
	(select avg (score)
	from score s2
	group by class
	having s1.class = s2.class);
~~~
~~~
+------+-------+
| name | score |
+------+-------+
| Taro |   100 |
| Siro |    40 |
+------+-------+
~~~

######うへぇ
