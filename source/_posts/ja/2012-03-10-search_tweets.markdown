---
layout: post
title: "ツイートを検索して表示してみよう"
date: 2012-03-10 17:35
comments: true
sharing: true
categories: MacRuby
---

TableView の使い方を [TableView を使ってみよう](/blog/2012/03/10/tableview-basic/) に書きました。今回は Twitter のツイートを検索して、TableView に表示するアプリケーションを作ってみようと思います。

以下のように、Text Field に検索ワードを入力し該当するツイートを TableView に表示します。

![image](/images/ja/search_tweets/search_tweets.png)


## 使用するアウトレットとアクションメソッド
使用するアウトレットは以下のとおりです。

```ruby
  attr_accessor :textField
  attr_accessor :tableView
```

Search ボタンがクリックされた時に呼び出されるアクションメソッドとして、以下のメソッドを用意します。

```ruby
  def search(sender)
    # 検索処理
  end
```


## アウトレットなどを接続
以下の図のように、アウトレットやアクション、データソースを接続します。

![image](/images/ja/search_tweets/connect_outlets_etc.png)


## Table カラムの Identifier を指定
Table のカラムを以下のように指定しておきます。

<table class="table">
<tr><th>カラム</th><th>identifier</th></tr>
<tr><td>1 列目</td><td>name</td></tr>
<tr><td>2 列目</td><td>text</td></tr>
</table>


## 検索処理を実装

Twitter API を用いて検索した際に、[https://dev.twitter.com/docs/api/1/get/search](https://dev.twitter.com/docs/api/1/get/search) という形式で JSON を取得できます。これを参考に実装します。

```ruby
require 'uri'
require 'json'

class AppDelegate
  attr_accessor :window
  attr_accessor :textField
  attr_accessor :tableView

  def search(sender)
    text = textField.stringValue  # Text Field から文字列を取得
    if text.length > 0
      query = URI.escape(text)    # 検索クエリーをエスケープ
      url = "http://search.twitter.com/search.json?q=#{query}&rpp=30"

      # HTTP アクセスし、レスポンスを取得
      nsurl = NSURL.URLWithString(url)
      str = NSString.stringWithContentsOfURL(nsurl,
                                             encoding: NSUTF8StringEncoding,
                                             error: nil)

      json = JSON.parse(str)
      @search_result = json['results']
      
      # TableView をリロード
      # numberOfRowsInTableView や tableView:objectValueForTableColumn:row:
      # が呼びだされます
      tableView.reloadData 
    end
  end

  def numberOfRowsInTableView(aTableView)
    return 0 if @search_result.nil?
    return @search_result.size
  end
  
  def tableView(aTableView,
                objectValueForTableColumn: aTableColumn,
                row: rowIndex)
    case aTableColumn.identifier
    when "name"
      return @search_result[rowIndex]['from_user']
    when "text"
      return @search_result[rowIndex]['text']
    end
  end
end
```

検索結果を取得する際に `NSString.stringWithContentsOfURL` を使用していますが、Ruby の `Net::HTTP` を使用することもできます。また、JSON の解析も Mac OS X 10.7 以降に限定すれば [NSJSONSerialization](https://developer.apple.com/library/mac/#documentation/Foundation/Reference/NSJSONSerialization_Class/Reference/Reference.html) を使うことができます。Ruby メソッドと Mac OS X API をいろいろ組み合わせて使用できますので、用途や利便性などで使いわけてみてください。
