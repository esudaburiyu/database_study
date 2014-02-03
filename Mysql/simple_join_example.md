# 表
~~~
select * from numbers;
+--------+
| number |
+--------+
|      1 |
|      2 |
|      3 |
|      4 |
|      5 |
+--------+
~~~

#準備
~~~
create table numbers(number int);
insert numbers (number) values (1);
insert numbers (number) values (2);
insert numbers (number) values (3);
insert numbers (number) values (4);
insert numbers (number) values (5);
~~~

# 順序
~~~
select * from numbers n1 join numbers n2 on n1.number < n2.number;
~~~
~~~
+--------+--------+
| number | number |
+--------+--------+
|      1 |      2 |
|      1 |      3 |
|      2 |      3 |
|      1 |      4 |
|      2 |      4 |
|      3 |      4 |
|      1 |      5 |
|      2 |      5 |
|      3 |      5 |
|      4 |      5 |
+--------+--------+
~~~

## ３つ
~~~
select * from numbers n1
join numbers n2 on n1.number < n2.number
join numbers n3 on n2.number < n3.number;
~~~
~~~
| number | number | number |
+--------+--------+--------+
|      1 |      2 |      3 |
|      1 |      2 |      4 |
|      1 |      3 |      4 |
|      2 |      3 |      4 |
|      1 |      2 |      5 |
|      1 |      3 |      5 |
|      2 |      3 |      5 |
|      1 |      4 |      5 |
|      2 |      4 |      5 |
|      3 |      4 |      5 |
+--------+--------+--------+
~~~


