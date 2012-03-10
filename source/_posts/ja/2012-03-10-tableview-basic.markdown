---
layout: post
title: "TableView を使ってみよう"
date: 2012-03-10 14:00
comments: true
sharing: true
categories: MacRuby
---

ここでは、TableView の使い方について説明します。TableView を使用するとデータをテーブル状に表示できます。この TableView はさまざまな Mac アプリケーションで見かけることができます。


## TableView を配置する
それでは、新規プロジェクトを作成し Table View を配置してみましょう。

![image](/images/ja/tableview-basic/tableview.png)


## データソースを接続する
TableView はデータソースという仕組みを用いて、データをテーブルに表示します。データソースは、UI 部品で表示する内容が変更したというイベントが発生すると、プログラム側からデータを提供してあげるという処理が行われます。アクションメソッドを接続したと同じように、どこから提供されるかを TableView に接続してあげる必要がります。

以下の図のように、<kbd>control</kbd> キーを押しながら TableView から App Delegate へドラッグします。

![image](/images/ja/tableview-basic/connect_datasource.png)

一覧に表示されている [dataSource] を選択して接続します。

![image](/images/ja/tableview-basic/datasource.png)

AppDelegate クラスからデータを提供するために、[NSTableViewDataSource](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/Reference/Reference.html) プロトコルの以下のメソッドを実装します。

```objc
- (NSInteger)numberOfRowsInTableView:(NSTableView *)aTableView
- (id)tableView:(NSTableView *)aTableView objectValueForTableColumn:(NSTableColumn *)aTableColumn row:(NSInteger)rowIndex
```

最初に `numberOfRowsInTableView:` メソッドが呼び出され、データが何行分あるのかを返します。
`tableView:objectValueForTableColumn:row:` メソッドが行数と列数に応じて呼び出されるので、テーブルに表示するデータを返してあげます。

```ruby
class AppDelegate
  attr_accessor :window

  def numberOfRowsInTableView(aTableView)
    # データが何行あるかを返す
    2
  end

  def tableView(aTableView, objectValueForTableColumn: aTableColumn, row: rowIndex)
    # テーブルに表示するデータを返す
    "foo"
  end
end
```

実行すると、以下のように表示されます。

![image](/images/ja/tableview-basic/tableview_sample1.png)


## セルごとに表示する内容を変える
上記のプログラムでは全てのセルで foo としか表示されないので、セルごとに表示するデータを変更してみましょう。

以下の図のように 1 列目の Table カラムを選択し、Identifier に name を設定します。

![image](/images/ja/tableview-basic/tableview_identifier.png)

同じように、今度は 2 列目の Table カラムを選択し、Identifier に age を設定します。

`tableView:objectValueForTableColumn:row:` メソッドを以下のように変更してみましょう。

```ruby
  def tableView(aTableView, objectValueForTableColumn: aTableColumn, row: rowIndex)
    # テーブルに表示するデータを返す
    # 2行 x 2列 = 4回、このメソッドが呼び出される
    if rowIndex == 0
      # 1行目のデータ
      case aTableColumn.identifier
      when 'name' # 1列目のデータ
        str = "Steven Paul Jobs"
      when 'age'  # 2列目のデータ
        str = "56"
      end
    end
      
    if rowIndex == 1
      # 2行目のデータ
      case aTableColumn.identifier
      when 'name' # 1列目のデータ
        str = "Stephen Gary Wozniak"
      when 'age'  # 2列目のデータ
        str = "61"
      end
    end
    
    return str
  end
```
rowIndex の値でどの行のデータが必要とされているのか判別できます。列は、aTableColumn.identifierの値によって判別できます。

![image](/images/ja/tableview-basic/tableview_sample2.png)
