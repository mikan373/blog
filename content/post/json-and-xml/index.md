---
title: 【比べてみた】JSONとXML🙉
description: 
slug: json-and-xml
date: 2022-06-17 00:00:00+0000
image: data.jpg
categories:
    - IT用語
tags:
    - JSON
    - XML
    - 比べてみた
---

こんにちは🌈  
Web APIではデータをJSON形式やXML形式で受け渡しします。  
それぞれどのような特徴があるのでしょうか？
***


### JSON
"JavaScript Object Notation"の略称です。
JSON形式ではデータが`{キー: バリュー}`のペアになっていることが特徴です。JavaScriptの記述と同様、波括弧がオブジェクトを、角括弧がリストを表します。  
下図は、お正月にやることを書いた例です。
```
{
    "date":"2022-01-01",
    "time":"12:00",
    "todo":["eat mochi", "play kite", "get otoshidama"]
}

```  

***
### XML
XML形式は"Extensible Markup Language"の略称で、W3Cによって作られました。タグの名前を自分で設定でき、自由度が高いのが特徴です。しかし、リスト内の要素も一つ一つタグで挟まなければならず、JSONと比べると全体的に可読性が下がっているように見えます。  
上記と同じ内容のデータを記載した場合、以下のようになります。
```
<note>
    <date>2022-01-01</date>
    <time>12:00</time>
    <todos>
        <todo>eat mochi</todo>
        <todo>play kite</todo>
        <todo>get otoshidama</todo>
    </todos>
<note>
```

***
### どっちを使うべきか？
こちらの記事によると、JavaScriptアプリケーションの人気が高まると同時に、JavaScriptと併用しやすいJSON形式のデータもより使われるようになったことや、開始タグと終了タグをその都度付けなくてもいいシンプルさから、近年ではJSONの方が人気のようです。  
しかしながら、会社内の複雑な要求を満たすアプリケーションにおいては、XMLの方が向いているケースもあるそうです。 

***
### おわりに
個人的な開発ではJSONしか扱ったことはないのですが、今回を機にXMLについて知ることができてよかったです。場面に合わせて適切な形式を選ぶようにしようと思います。


> 参考リンク
> - [Introduction to XML - W3Schools ](https://www.w3schools.com/xml/xml_whatis.asp)
> - [A Deep Look at JSON vs. XML, Part 1: The History of Each Standard](https://www.toptal.com/web/json-vs-xml-part-1)