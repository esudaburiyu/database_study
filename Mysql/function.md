#実験に使うテーブル


#実験に使うテーブルを用意する
~~~
create database score;
use score
create table score(id int, name varchar(255), class int, math int, eng int, ja int);
insert into score (id, name, class, math, eng, ja) values (1, 'Ichiro', 1, 90, 90, 90);
insert into score (id, name, class, math, eng, ja) values (2, 'Jiro', 1, 80, 80, 80);
insert into score (id, name, class, math, eng, ja) values (3, 'Sabu', 1, 70, 70, 70);
insert into score (id, name, class, math, eng, ja) values (4, 'Siro', 2, 90, 90, 90);
insert into score (id, name, class, math, eng, ja) values (5, 'Goro', 2, 60, 60, 60);
~~~

#Count
~~~
select count(*) from score;
~~~
結果
~~~
+----------+
| count(*) |
+----------+
|        5 |
+----------+
~~~

##Group Byを併用してみる
~~~
select class, count(*) from score group by class;
~~~
結果
~~~
+-------+----------+
| class | count(*) |
+-------+----------+
|     1 |        3 |
|     2 |        2 |
+-------+----------+
~~~

#sum
~~~
select sum(eng) from score;
~~~
~~~
+----------+
| sum(eng) |
+----------+
|      390 |
+----------+
~~~

##Group Byを併用してみる
~~~
select class, sum(eng) from score group by class;
~~~
~~~
+-------+----------+
| class | sum(eng) |
+-------+----------+
|     1 |      240 |
|     2 |      150 |
+-------+----------+
~~~

#avg
~~~
select class, avg(eng) from score group by class;
~~~
~~~
+-------+----------+
| class | avg(eng) |
+-------+----------+
|     1 |  80.0000 |
|     2 |  75.0000 |
+-------+----------+
~~~

#havingを使ってみる
~~~
select class, avg(eng) from score group by class having avg(eng)>77;
~~~
~~~
+-------+----------+
| class | avg(eng) |
+-------+----------+
|     1 |  80.0000 |
+-------+----------+
~~~

