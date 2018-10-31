---
title: "ABS"
category: Users/tk/competitive/AtCoder
tags: 
created_at: 2018-08-29 21:12:51 +0900
updated_at: 2018-08-31 20:24:07 +0900
published: true
number: 46
---

# [PracticeA - はじめてのあっとこーだー（Welcome to AtCoder）](https://beta.atcoder.jp/contests/abs/tasks/practice_1)

## 提出コード

```python:my_submit.py
# coding=utf-8

A = int(input())
D = str(input())
S = str(input())

D_SPLIT = D.split()
B = int(D_SPLIT[0])
C = int(D_SPLIT[1])

print('{} {}'.format(A + B + C, S))
```

## 正解コード

```python:answer.py
a = int(input())
b, c = map(int, input().split())
print("{} {}".format(a+b+c, s))
```

## 詰まったところ
### 2行目の処理
#### 概要
2行目の入力がスペースで区切って2つあるので、その処理をスムーズにできなかったので以下の手順に分解して実装した。

1. 2行目の入力`b c`を文字列として受け取る
1. それを`split()`でスペース区切りで分けてリスト化する
1. 文字列のリストを数値のリストに変換しつつひとつずつ取り出す

実装しながら「絶対違うよなこれ効率悪すぎ」と思っていたが、上記の手順の実装すら怪しかったので諦めた。

#### 解決策
map関数を用いたら一行で実装できた。

```python:map.py
b, c = map(int, input().split())
```

map関数の使い方：
```
map(func, iterable, ...)
```
> 第2引数以降のiterableを順にfuncに渡した結果をyieldで返す(generator)。
引数iteratbleは第1引数の関数の引数に順に渡される。
>
>Python2系では，上に挙げたそれぞれの関数、メソッドはそれぞれリストを返している。
しかし，Python3系ではリストではなく、iterator objectを返すように挙動が変更された。
iteratorを返すことで、一度に全要素を計算せず、要素が必要とされる時に計算するようになり、使用するメモリ量を低く抑えているとのこと。
>
>2系のように、iteratorをリスト化したい場合はiteratorをlist関数に渡してリストを生成すればよい。

```python:map.py
map(square, range(1, 10))
# <map at 0x7f474ad63e80>

list(map(square, range(1, 10)))
# [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

イテレーターとかiterableとかiterator objectってなんやねん。
→明日調べてまとめる
参照先候補リスト
**わかりやすそう**　　
http://nihaoshijie.hatenadiary.jp/entry/2016/11/21/221220
**ついでにジェネレーターについても学んどこう**
http://nihaoshijie.hatenadiary.jp/entry/2018/07/12/140540

では、先程の解法のように使用するときの挙動はどうなってるのか。

```python:map_experiment.py
list = [1, 2, 3]
a, b, c = map(lambda x: x ** 2, list)
print(a, b, c)
# 1 4 9

# 3 vs 5 (left < right)
list2 = [1, 2, 3, 4, 5]
a2, b2, c2 = map(lambda x: x ** 2, list2)

# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# ValueError: too many values to unpack (expected 3)

# 4 vs 3 (left > right)
aa, bb, cc, dd = map(lambda x: x ** 2, list)
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# ValueError: not enough values to unpack (expected 4, got 3)
```
→どうやら代入する変数とiterableの要素数（イテレーション数）が一致しないとエラーを吐くらしい。

## 調べたこと
- input()でデータ型を指定するとき：  
`int(input())`など
- split()の使い方：  
`split(',')`→コンマで区切る
- 文字列リストを数値のリストに変換：  
`list_b = [int(n) for n in lsit_a]`

## その他メモ

## 参照
- [Python3におけるmap/filterの使い方](http://hiroto1979.hatenablog.jp/entry/2016/02/24/020534)
