---
title: PythonでSQLiteを操作する方法
description: 
slug: python-sqlite
date: 2022-07-20 00:00:00+0000
image: pexels-tatiana-syrikova-3975585.jpg
categories:
    - DB
tags:
    - Python
    - SQLite
---

こんにちは🧋  

SQLiteはオープンソースの軽量なデータベースです。  
今回はPythonでSQLiteを操作する方法をまとめていきます。

***


### 使用する基本のメソッド
SQLiteを操作するために、以下のメソッドを使用します。
>- sqlite3`.connect(dbname)`…SQLiteへ接続
>- conn`.cursor()`…SQLiteを操作するためにカーソルオブジェクトを作成する。
>- cur`.execute(SQL文)`…SQL文を実行する。
>- conn`.commit()`…処理を反映する。
>- conn`.close()`…SQLiteとの接続を閉じる  

### データベースとテーブルの作成
試しに、companyというデータベースの中に、employeesテーブルを作って行きたいと思います。
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute('CREATE TABLE employees(id INT PRIMARY KEY AUTOINCREMENT, name TEXT, age INT, department TEXT)')

conn.commit()
conn.close()
```
これを実行するとデータベースとテーブルが作成できました。  

### 行を挿入する
SQL文を`cur.execute()`の括弧内に入れることで、各種処理を実行できます。  
以下のように、行を挿入してみます。
さきほどの例では文を横一列に書きましたが、`'`の代わりに`'''`で囲むことで、複数行にわたって書くことができます。
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute(
    '''
    INSERT INTO 
        employees(name, age, department)
    VALUES
        ("ichiro", 50, "IT")
    '''
)

conn.commit()
conn.close()
```

### 変数を使って行を挿入する
挿入したい値が変数の場合は、SQL文上でその場所を`?`にしておき、`.execute()`の**第2引数**として変数名をいれます。  
以下では、`department`という項目に入れる値が、変数`deptname`であった時の例です。第2引数はタプルである必要があるので、下記のように値が一つだけの場合は最後に`,`が必要です。
```python
import sqlite3

deptname = "Human Resources"

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute(
    '''
    INSERT INTO 
        employees(name, age, department)
    VALUES
        ("jiro", 32, ?)
    ''',
    (deptname,)
)

conn.commit()
conn.close()
```

***
### データを取得する
データを取得する時は、カーソルオブジェクト(本記事では変数"cur")に以下のようなメソッドを適用します。

>- cur`.fetchone()`…条件に一致する行を一つだけ取得
>- cur`.fetchall()`…条件に一致する全ての行を取得

以下は`.fetchall()`を使用した例です。
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute('SELECT * FROM employees')

print(cur.fetchall())

conn.commit()
conn.close()
```
print結果は以下のようになります。
```
[(1, 'ichiro', 50, 'IT'), (2, 'jiro', 32, 'Human Resources')]
```

### 列名を指定してデータを取得する
コネクションオブジェクトの`row_factory`属性を`sqlite3.Row`型に設定しておくと、列の名前を指定してデータを取得することが可能です。
>- conn.row_factory = sqlite3.Row

そしてそのあと、カーソルオブジェクトからデータを取り出す時に、forループで各行の列名を指定します。
>- for row in cur:  
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(row["列名"])

使用例は以下の通りです。
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)

conn.row_factory = sqlite3.Row

cur = conn.cursor()
cur = conn.execute('SELECT * FROM employees')

for row in cur:
    print(row["name"])

conn.close()
```
print結果は以下のようになりました。
```
ichiro
jiro
```

### 公式ドキュメント
docs.python.orgに詳細な記載があります。
[sqlite3 --- SQLite データベースに対する DB-API 2.0 インタフェース](https://docs.python.org/ja/3.8/library/sqlite3.html)
