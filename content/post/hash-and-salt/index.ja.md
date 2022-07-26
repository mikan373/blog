---
title: パスワードは取り扱い注意🚨 ハッシュ・ソルトとは？
description: 
slug: hash-and-salt
date: 2022-06-08 00:00:00+0000
image: hash.jpg
categories:
    - Security
tags:
    - パスワード
    - ハッシュ
    - bcrypt
    - Python
---

こんにちは🌱  
今回はデータベース上でパスワードをどのように保存したらいいか？について、調べたことをまとめたいと思います。  
<br />
　
### パスワードをそのままDBに保存するのはご法度！！
ユーザー登録をする際、ユーザー名とパスワードを設定するのが一般的ですよね。  
登録したデータはサーバーに送られ、データベースに保存されます。  
  
しかし、もしこのデータベースの情報が悪意のある第三者に盗まれてしまったら...

> 😎🗯ユーザーネームとパスワードを使って不正にログインするぞ〜〜


こんなことが起きたら大変ですよね。  
パスワードをそのままの文字列で保存しておくことは、万が一データベースの情報が盗まれた場合、大変危険です。  
ここで言う「そのまま」の文のことを**平文**\(**ひらぶん**\)と言います。  

では、平文でデータベースに保存しないためにはどうすれば良いのでしょうか。  
<br />
***

### ハッシュ化とは
DB上でパスワードをセキュアに保存するときによく使われるのは、**ハッシュ化**です。  
　　　
#### いきなりまとめ
- ハッシュ化とは、数学的アルゴリズム(ハッシュ関数)を使用して、データを不規則な文字列に変換する技術のこと。
- ハッシュアルゴリズムにはMD5やSHA256などいくつも種類がある。
- ハッシュ化された値から元データに戻すことはできない。（極めて困難）
- 同じデータからは同じハッシュ値が生成される。  
<br>
  
#### ためしてみる
今回はSHA256というアルゴリズムを使ってPythonで実際にハッシュ値を求めていきたいと思います。  
Pythonでハッシュ値を求める場合は、まず`hashlib`という標準ライブラリをインポートします。  
そして、ハッシュ化したい元データを`password`とし、それを`encode()`して`hashlib.sha256`の引数に指定します。  
💡エンコードとは文字列をバイト列に変換することです。  
さらに`.hexdigest()`で16進数化します。
```python {.scroll}
>>> import hashlib
>>> password = "apple"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
```
print結果は...以下のような英数字の羅列になります。
```
3a7bd3e2360a3d29eea436fcfb7e44c735d117c42d1c1835420b6b9942dd4f1b
```

これで、元データがなんだったか全く分かりませんね。  
  
今度は違う元データを使ってみましょう。  
```python
>>> password = "りんご"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
```
結果はこちらです。
```
4261abfc91324dc5319312592125610a16b0b0a996fcdfae1d24766b918afae9
```
違うデータでは異なる文字列が返ってきましたね。  
ところで、ハッシュ値が異なる一方で、値の長さはどちらも64桁でした。
このように、異なる長さのデータでも固定長のデータに変換されることもハッシュ化の特徴です。  
***
### パスワードをハッシュで保存する際の問題点
ハッシュ化が便利な事は分かりましたが、これをパスワードを保存・管理する場面で使うには実は問題があります。  

例えば、Aさんがあるアプリでユーザー登録をしたとします。  
このとき、Aさんはパスワードを考えるのが面倒で、パスワードを"password123"として登録しました。  
そして偶然にも、BさんやCさんも、全く同じ"password123"をパスワードとして使っていました。  
すると、どうでしょう。  

>👩 "password123"<br>  
>👨 "password123"<br>  
>👵 "password123"<br>  

3人のパスワードはハッシュ化されて保存されましたが、元のパスワードが同じであるためハッシュ値も同じ文字列となります。  
つまり、この3人が同じ文字列をパスワードとして使っていることが判断できてしまうのです。  この状態だと、一人のパスワードが漏洩した時点で全員のパスワードがわかってしまいます。 

>👩 "password123"  →  "ef92b778bafe771e..."<br>  
>👨 "password123"  →  "ef92b778bafe771e..."<br>   
>👵 "password123"  →  "ef92b778bafe771e..."<br>    
>😎💭 この３人は同じパスワードを使っているな。ふむふむ  
 

攻撃者の中には総当たり攻撃を仕掛けてパスワードを類推しようとする人もいます。  
一部のユーザーのパスワードが漏洩したり、攻撃を受けたりしても問題ないように、何か対策を取りたいところですよね。  

***
### ソルトとは

一つの方法として、パスワードを**ソルト**することが挙げられます。  

>😧💭 ソルト...って、え、塩🧂...?


はい、英語でも"salt"と書きますから、パスワードに塩を振りかけるイメージです。（※個人の見解です） 


#### いきなりまとめ
 - ソルトとは、元データに付け加えるランダムな文字列のこと。
 - 文字列はパスワードごとに異なるものを使用する。
 - ソルトを元データに追加してからハッシュ化することで、同じ元データをハッシュしても異なる文字列が生成される  

これならより安全にパスワードを保存することができますね。

### bcryptを使う
[bcryptというライブラリ](https://pypi.org/project/bcrypt/)を使うと簡単にソルト＆ハッシュしたパスワードを生成できるので、早速pip installして使ってみましょう。使い方も上記のリンク先に記載されています。  
#### つかってみる
- `bcrypt.gensalt()`...ランダムなソルトを生成する。  
- `bcrypt.hashpw([password],[salt])`...パスワードにソルトを付加してハッシュ化する。



上記を組み合わせて実際に使ってみましょう。
```python
>>> import bcrypt
>>> password = "opensesame01"
>>> hashed_and_salted = bcrypt.hashpw(bytes(password, 'utf-8'), bcrypt.gensalt())
```
 
これで、`hashed_and_salted`をprint()してみると...
```python
>>> print(hashed_and_salted)
b'$2b$12$j6xy952qXR3xCYa8SOpGiewm5DXAZ/YNfbgJHcwuOkjml7jZDkxUi'
```
ソルトした上でハッシュ化された文字列が出来上がりました。  
ちなみに、`.hashpw()`でハッシュ化する際に、第1引数を`bytes(password, 'utf-8')`としているのは、文字列をバイト列に変換するためです。 
文字列をバイト列に変換する方法は他にも`encode([string])`や`b"[string]"`という書き方があります。これらはデフォルトでUTF-8に変換されます。  
  

ここでちょっと試しに、**ただハッシュしただけ**の場合と比べてみましょう。
```python
>>> import hashlib
>>> password = "opensesame01"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
'4b368659a35f63cef458a1811e46513da66e62ca27c9a31f613bd62189b0e1ed'
```
全く違う文字列が出来上がっていますね！  
ナイスです。  

次に、ソルトした上でハッシュ化された文字列が、元々のパスワードと一致しているのか検証してみます。  
検証するときは、下記のように第1引数に検証したい文字列、第2引数に元々のパスワードをhashed & saltedした文字列を入れます。  
- `bcrypt.checkpw([password],[hashed_password])`

実務では入力されたパスワードが、データベースにパスワードとして保存されている文字列と一致するか検証するために使うと思うので、第1引数の名前は`entered_password`にしてみました。⬇️
```python
>>> entered_password = "opengoma01"
>>> bcrypt.checkpw(bytes(entered_password, 'utf-8'), hashed_and_salted)
False
>>> entered_password = "opensesame01"
>>> bcrypt.checkpw(bytes(entered_password, 'utf-8'), hashed_and_salted)
True
```
ちゃんと、間違っている文字列では`False`が、正しい文字列では`True`が返されました！  
> 👩👨👵 わーい！  

***  
### パスワードをhash & salt してセキュアに保存しよう！

今回はパスワードをソルトを加えてハッシュ化する方法についてまとめました。  
パスワードの基本の扱い方として、よく覚えておきたいと思います。  
  
### 参考リンク
>- [hashlib — Secure hashes and message digests](https://docs.python.org/3/library/hashlib.html)  
>- [Hashing Algorithm Overview: Types, Methodologies & Usage](https://www.okta.com/identity-101/hashing-algorithms/#:~:text=A%20hashing%20algorithm%20is%20a,and%20decoded%20by%20anyone%20else.)

🍔🧂🍟
