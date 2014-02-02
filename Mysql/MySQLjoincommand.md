~~~
create database jointest;
create table big(id int, text varchar(255));
create table small(id int, text varchar(255));
insert into big (id,text) values (1,'AAAAA');
insert big (id, text) values (2, BBBBB);
insert big (text, id) values ('CCCCC', 3);
~~~
######into