#ポイント

|演算の名前| 記号 | SQL | MySQL|
|---|---|---|---|
|和 | ∪ | UNION | UNION |
|差| - | EXCEPT | except|
|共通| ∩ | INTERSECT | INTERSECT|
|直積|× | CROSS JOIN | CROSS JOIN |
|射影|?| SELECT | SELECT |
|選択| ? | WHERE | WHERE |
| 結合 |?  | ?| ? |
|商 | ÷ | なし | なし |

#関係演算とは
関係演算には、

- 和
- 差
- 共通
- 直積

のような普通の集合演算に加えて、

- 射影
- 選択
- 結合
- 商

がある。

# 和、差、共通について
こいつらって、関係Rと関係Sが同じようなスキーマの時にやれるんだよね?

それぞれ R ∪ S, R - S, R ∩ S

SQLでは、UNION , EXCEPT, INTERSECT
として実装されている。


# 直積
これを基本

SQLではCROSS JOIN

# 射影
列を抜き出す。SELECT。

# 選択
行を抜き出す。 WHERE

# 結合
直積の変形?

#商
SQLでは直接扱わない?
複雑に命令を組み合わせれば実現できるみたい。

# HAVINGとかGROUP BY、SUMとかはどういう位置づけになるんだっけ?
[Wikipedia](http://ja.wikipedia.org/wiki/%E9%96%A2%E4%BF%82%E4%BB%A3%E6%95%B0_(%E9%96%A2%E4%BF%82%E3%83%A2%E3%83%87%E3%83%AB)#.E5.BF.9C.E7.94.A8.E7.9A.84.E3.81.AA.E6.BC.94.E7.AE.97.E5.AD.90)

sumなどは、要約、という分類。数学的に純粋な関係代数には含まれない?
