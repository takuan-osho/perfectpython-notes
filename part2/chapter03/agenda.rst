型とリテラル
============

この第3章で扱われている項目は以下の通りです。

#. オブジェクトについて
#. 論理型
#. 数値型
#. シーケンス(Sequence)
#. set(セット)
#. 辞書型(Dictionaries)
#. None型

::

   「本の引用」と「著者の見解・考え」を明確にするために、以下の文章では「本の引用」またはそれに準ずる内容の記述箇所については整形済みブロック内に書くことにします。


オブジェクトについて
--------------------

個人的にポイントだと思うところ

::

   Pythonの場合は、オブジェクトのリファレンスにあらかじめ型を指定する必要がありません。


論理型
------

*個人的に大切だと思っている点*

- True(真)とFalse(偽)の2つの論理値を持つこと。
- TrueとFalseは予約語であること。

特に2点目は大切。というのも、Python2.xではTrueもFalseも予約語ではなかったから。

そのため、Python2.xだとTrueやFalseに別の値を代入できてしまい、TrueやFalseの挙動を無理矢理変えることもできてしまいます。そんなトリッキーなプログラムを普段書くことはないと思いますが。


シーケンス(Sequence)
--------------------

簡単に言うと、他の言語で言うところの「配列」や「イテレータ」などのこと。より正確な表現は本を参照。

文字列・バイト列
^^^^^^^^^^^^^^^^

Python3.x系になり、マルチバイト文字列の扱いもやりやすくなりました。

- 文字列型はstrとなり、内部でUnicodeデータを保持するようになったこと
   - Python2.x系で初心者がよく嵌まるUnicodeDecodeErrorやUnicodeEncodeErrorに悩まされにくくなった(はず)
- スクリプトファイルがUTF-8で記述されていることを想定するようになったこと

ファイルの文字コードの設定は普段からやっておいた方が面倒が少なくなると個人的には思っています。

バイト列を扱うときにはbytes型を使用します。

::

   x = b"These are some bytes"

イミュータブル
""""""""""""""

Python3ではstr型もbytes型もともにイミュータブル(変更不能)オブジェクトです。

バイト配列型
^^^^^^^^^^^^

ミュータブルなバイト列を扱うときはbytearray(バイト配列型)を使用します。

リスト(lists)
^^^^^^^^^^^^^

順序を保ったままのオブジェクトのリストを扱うときに使用します。Javaなどの配列と違い、Pythonのlistは複数種類の型のオブジェクトを一つのリストに入れることが出来ます。

::

   >>> x = [1, 2, 3.0, "a", "b", "c"]

Pythonのlistはミュータブル(変更可能)です。イミュータブルなリストみたいなものを使いたい場合はtupleを使ってみるといいと思います。

インデックシング(シーケンス番号を使ったアクセス)
""""""""""""""""""""""""""""""""""""""""""""""""

::

   >>> x = [1, 2, 3.0, "a", "b", "c"]
   >>> x[0]
   1
   >>> x[1]
   2
   >>> x[5]
   'c'
   >>> x[6]
   Traceback (most recent call last):
     File "<input>", line 1, in <module>
   IndexError: list index out of range

インデックスを使ってアクセスできます。インデックスは0始まりで、範囲外のインデックスを使用するとIndexErrorが返されます。

最後の要素にアクセスしたい場合はネガティブインデックスと呼ばれる方法で可能です。

::

   >>> x = [1, 2, 3.0, "a", "b", "c"]
   >>> x[-1]
   'c'
   >>> x[-6]
   1
   >>> x[-7]
   Traceback (most recent call last):
     File "<input>", line 1, in <module>
   IndexError: list index out of range

こちらも範囲外のインデックスを使用するとIndexErrorが返されます。

スライス
""""""""

スライスはlistからサブリストを得るためのシンタックスです。

::

   >>> x[1:2]
   [2]
   >>> x[1:4]
   [2, 3.0, 'a']
   >>> x[2:-1]
   [3.0, 'a', 'b']
   >>> x[-3:-1]
   ['a', 'b']
   >>> x[-1:-3]
   []
   >>> x[-3:-7]
   []

使い方は上記のような感じです。ネガティブインデックスも使用可能で、スライスの指定がマッチしなかった場合には空のlistが帰ります。

開始位置や終了位置を示すインデックスを省略も出来ます。省略された場合は戦闘と末尾が自動で適用されます。

::
    
   >>> x[1:]
   [2, 3.0, 'a', 'b', 'c']
   >>> x[:3]
   [1, 2, 3.0]
   >>> x[:-2]
   [1, 2, 3.0, 'a']
   >>> x[:]
   [1, 2, 3.0, 'a', 'b', 'c']

イテレーション
""""""""""""""

通常のループを使ってlistの全要素にアクセスできます。

::

   >>> for item in x:
   ...     print(item)
   ...
   ...
   1
   2
   3.0
   a
   b
   c

リストの更新
""""""""""""

Pythonのlistはミュータブルです。以下のようなメソッドを使ってリストを更新できます。

- append
- remove
- reverse
- sort

reverseメソッドもsortメソッドもlistを書き換えるので注意してください。

::

   >>> x = [1, 2, 3, 4, 5]
   >>> x.reverse()
   >>> x
   [5, 4, 3, 2, 1]

   >>> x = [4, 5, 2, 0, 7, 9 ,3, 2, 5, 5, 6]
   >>> x.sort()
   >>> x
   [0, 2, 2, 3, 4, 5, 5, 5, 6, 7, 9]

listを変更するのにスライスも使えます。

::

   >>> x = [1, 2, 3, 4, 5]
   >>> x[1:4]
   [2, 3, 4]
   >>> x[1:4] = ["spam", "eggs"]
   >>> x
   [1, 'spam', 'eggs', 5]

スライスの置き換えは要素数が違っていても可能です。
