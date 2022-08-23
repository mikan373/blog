---
title: Differences between JSON and XML 🙉
description: 
slug: json-and-xml
date: 2022-06-17 00:00:00+0000
image: data.jpg
categories:
    - IT-term
tags:
    - JSON
    - XML
    - 比べてみた
---

Hello🌈  
Web API transfers data in JSON or XML format.
What are the differences of these two?
***


### JSON
JSON stands for "JavaScript Object Notation".
In JSON, data is wrriten as `{key: value}` pair.
Just as same as JavaScript, curly brackets enclose objects, and square brackets enclose lists.  
Following exapmle is a to-do list on New Year's Day written in JSON.
```
{
    "date":"2022-01-01",
    "time":"12:00",
    "todo":["eat mochi", "play kite", "get otoshidama"]
}

```  

***
### XML
SML stands for "Extensible Markup Language". It was created by W3C and you can decide the tag name by yourself. However you need to use tagsfor  every elements in the list, which is tiresome. Its readability gets less too.  
If you write the same data in XML, it would look like this.
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
### Which one should I use?
According to [this article](https://www.toptal.com/web/json-vs-xml-part-1), as JavaScript application gets more popular, more people started to use JSON data which goes well with JavaScript. 
の記事によると、JavaScriptアプリケーションの人気が高まると同時に、JavaScriptと併用しやすいJSON形式のデータもより使われるようになったことや、開始タグと終了タグをその都度付けなくてもいいシンプルさから、近年ではJSONの方が人気のようです。  
しかしながら、会社内の複雑な要求を満たすアプリケーションにおいては、XMLの方が向いているケースもあるそうです。 

***
### おわりに
個人的な開発ではJSONしか扱ったことはないのですが、今回を機にXMLについて知ることができてよかったです。場面に合わせて適切な形式を選ぶようにしようと思います。


> 参考リンク
> - [Introduction to XML - W3Schools ](https://www.w3schools.com/xml/xml_whatis.asp)
> - [A Deep Look at JSON vs. XML, Part 1: The History of Each Standard](https://www.toptal.com/web/json-vs-xml-part-1)