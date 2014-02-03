#表の結合 ポイント
- 表の結合は、大きく分けて、**内部結合**と**外部結合**に分かれる。
- 内部結合と外部結合は、どちらも2つの表を、共通する列に基いて結合する。
- 内部結合と外部結合の違いは、片方にはないレコードを、無視するか、NULLを入れるか、ということ

# 内部結合のポイント
- 等結合は、Θ結合のうち、条件に等号を使ったもの
- 自然結合は、等結合の結果から、結合に使ったフィールドの一方を削ったもの

##Θ結合
~~~
SELECT *
FROM A INNER JOIN B
ON A.id  Θ B.id
~~~

##等結合
~~~
SELECT *
FROM A INNER JOIN B
ON A.id = B.id
~~~

##自然結合
######自然って、なんか難しい理論で言う自然性と関係あるの???

#外部結合のポイント
外部結合には、

- 左外部結合
- 右外部結合
- 完全外部結合

の三種類がある。NULLの入れ方が違うだけ。

#例
テーブル名: ひらがな

|id|ひらがな|
|---|---|
|1|ああああああああああああ|
|2|かかかかかかかかかかかか|
|3|ささささささささささささ|
|4|たたたたたたたたたたたた|


テーブル名:英語

|id|英語|
|---|---|
|1|AAAAAAAAAAAAAAAAA|
|2|BBBBBBBBBBBBBBBBBBBBB|
|3|CCCCCCCCCCCCCCCCC|
|5|EEEEEEEEEEEEEEEEEEEEE|

# 内部結合
	SELECT *
	FROM ひらがな join 英語 on ひらがな.id = 英語.id
とすれば

|id|ひらがな|英語|
|---|---|---|
|1|ああああああああああああ|AAAAAAAAAAAA|
|2|かかかかかかかかかかかか|BBBBBBBBBBBBBBB|
|3|ささささささささささささ|CCCCCCCCCCCC|

みたいにでてくるはず
#外部結合
##左結合
	SELECT *
	FROM ひらがな left join 英語 on ひらがな.id = 英語.id
###結果
|id|ひらがな|英語|
|---|---|---|
|1|ああああああああああああ|AAAAAAAAAAAA|
|2|かかかかかかかかかかかか|BBBBBBBBBBBBBBB|
|3|ささささささささささささ|CCCCCCCCCCCC|
|4|たたたたたたたたたたたた|NULL|

##右結合
	SELECT *
	FROM ひらがな right join 英語 on ひらがな.id = 英語.id

|id|ひらがな|英語|
|---|---|---|
|1|ああああああああああああ|AAAAAAAAAAAA|
|2|かかかかかかかかかかかか|BBBBBBBBBBBBBBB|
|3|ささささささささささささ|CCCCCCCCCCCC|
|5|NULL|EEEEEEEEEEEEEEE|

######A left join Bと、B right join Aって、おんなじもの?それなら、left joinとright joinの両方って必要?

##完全結合
|id|ひらがな|英語|
|---|---|---|
|1|ああああああああああああ|AAAAAAAAAAAA|
|2|かかかかかかかかかかかか|BBBBBBBBBBBBBBB|
|3|ささささささささささささ|CCCCCCCCCCCC|
|4|たたたたたたたたたたたた|NULL|
|5|NULL|EEEEEEEEEEEEEEE|

