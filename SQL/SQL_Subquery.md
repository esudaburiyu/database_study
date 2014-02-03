#参考URL
[高度な副問い合わせ](http://www.atmarkit.co.jp/ait/articles/1209/14/news146.html)

#SQLの相関副問い合わせとは

#副問い合わせと相関副問い合わせの違い
単純な副問い合わせでは、subqueryの値が、mainqueryのなかで変わらない。

しかし、相関副問い合わせでは、subqueryの値が、mainqueryのなかで行ごとに異なる。

#単純副問い合わせか、相関副問い合わせか、SQL上で見分けるポイントは?
~~~
select name, score
from scoresheet
where score >
	(select avg(score)
	from score);
~~~
######この例だと、副問い合わせの結果が、一行になる。

~~~
select name, score
from scoresheet s1 where score >
	(select avg (score)
	from scoresheet s2
	group by class
	having s1.class = s2.class);
~~~