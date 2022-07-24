---
title: HTTP通信ってステートレスだけどどうやって状態を管理するの？
description: Cookieとセッションについてのまとめ
slug: session-and-cookie
date: 2022-06-22 00:00:00+0000
image: cookie.jpg
categories:
    - HTTP
tags:
    - セッション
    - Cookie
---

こんにちは👋🏻  

HTTP通信の特徴の一つに、ステートレスであることが挙げられます。  
ステートレスであるということは、クライアントとサーバーのやりとりは個々に独立したものであり、関連性を持たないということです。  

でも、個々のやりとりが独立していたら、どうやってそれまでの操作した状態を保存しておくことができるのでしょうか？例えば、ECサイトで買い物カゴにアイテムを入れたあと、決済画面に進んだときアイテムが消えていたら困ってしまいますよね。  

***

### Cookieとは？
まずCookieというものについてお話しします。🍪  
Cookieとは、WebサーバーがWebブラウザに与える小さなファイルのことです。ユーザー情報やカートに入れた商品の情報など色々なデータを保存しておくことができます。Cookieは、以下のような流れで使用されています。

1. Webサーバーは、あるWebブラウザから初めてアクセスがあった時に、HTTPレスポンスのヘッダーに`Set-Cookie`という項目を含めて送信することで、そのWebサーバー専用のCookieファイルをWebブラウザ上に作成する。  
2. WebブラウザはCookieを自身に保存しておき、次回以降そのWebサーバーにHTTPリクエストを送る際は、リクエストヘッダーに、1.で保存しておいた`Cookie`を含めて送信する。

WebブラウザがCookieを保持しておき、通信のたびにCookieをWebサーバーに送ることでそれまでのデータを渡すことができます。  

ちなみにCookieがなぜCookieと呼ばれるかには諸説理由があるようです。気になる方は[こちら](https://www.lotte.co.jp/kengaku/biscuit/topics/topics06.html)をご覧ください。（私は気になってしまいました…🍪）

***
### 状態の管理に使われる「セッションID」
さて、では最初の問題に戻ります。WebブラウザとWebサーバーのリクエストとレスポンスは個々に独立したやりとりですが、どうやって状態を管理するのでしょうか？  

結論から書きますと、これは、**セッションID**というものをCookieに含めて保存することで実現されています。  

**セッション**とは、WebブラウザとWebサーバーの通信が始まってから終わるまでのやりとりのことです。例えば、ECサイトでいうと、「ログイン→商品選択→カートに入れる→購入する→ログアウト」までの一連の流れをセッションと呼ぶことができます。

このセッションを管理するためには、**セッションID**が使われます。Webサーバーは同時にたくさんのクライアントと通信しているため、そのセッションがどのクライアントととの通信かを判別するために、それぞれにセッションIDを付与します。そしてセッションIDは**Cookie**に含まれてWebブラウザに送信され、Webブラウザはそれを保存します。  

2回目以降に通信を行う際、WebブラウザはWebサーバー宛にセッションIDを含んだCookieを送信します。Webサーバーは受け取ったセッションIDを元に、そのリクエストがどのセッションの続きか判断できるようになるのです。  

このようにして、HTTPはステートレスでありながら、ステートフルな通信ができるようになります。

### おわりに
自分が使っているWebサービスやアプリを思い浮かべてみると、ログイン状態や項目を選択した状態を保持できなかったら成り立たないものばかりですね。。。セッション管理の重要性がよく分かりました。