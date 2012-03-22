---
layout: post
title: "RubyGems を利用して Markdown ファイルを表示しよう"
date: 2012-03-22 16:30
comments: true
sharing: true
categories: MacRuby
---

MacRuby は WebKit など Objective-C で作られたフレームワークはもちろん、RubyGems ライブラリを用いてアプリケーションを作ることができます。今回は、Markdown ファイルを HTML へ変換する RubyGems ライブラリを利用し、WebKit で変換された HTML を表示するアプリケーションを作成します。


## RubyGems ライブラリをインストールする
Markdown ファイルを HTML へ変換するために、[rdiscount](http://rubygems.org/gems/rdiscount) という RubyGems を用います。Markdown ファイルを変換するためのライブラリはほかにも [BlueCloth](http://deveiate.org/projects/BlueCloth) などいくつかありますので、いろいろ試してみるとおもしろいかもしれません。

それでは、rdiscount をインストールしてみましょう。Terminal 上で以下のようにコマンドを実行します。

```
$ sudo macgem install rdiscount
```

rdiscount を用いて以下のように、`RDiscount.new` へ Markdown ファイルの内容を渡し `to_html` というメソッドで HTML へ変換できます。

```
$ macirb
irb(main):001:0> require 'rubygems'
=> true
irb(main):002:0> require 'rdiscount'
=> true
irb(main):003:0> path = "MarkDownSyntax.md"
=> "MarkDownSyntax.md"
irb(main):004:0> md = RDiscount.new(File.read(path))
=> #<RDiscount:0x4008cace0 以下略
irb(main):005:0> md.to_html
=> "<h1>Markdown: Syntax</h1>\n\n<ul 以下略
```

<div class="note">
CRuby 1.9 では <code>require 'rubygems'</code> が不要なのですが、MacRuby で RubyGems を利用する際には必須となります。「MacRuby の起動にさらに時間がかかるので、rubygems をロードしていない」というのが理由です。(参考: <a href="http://www.macruby.org/trac/ticket/855">MacRuby should load the "rubygems" automatically.</a>)
</div>


## Web Viewの準備
[WebKit フレームワークを使ってみよう](/blog/2012/03/15/webkit/)の記事を参考に、Web View をアウトレット接続するところまでを行います。

![image](/images/ja/webkit/connect_outlet.png)


## Markdown ファイルを読む
アプリケーションメニューの [File]->[Open...] がクリックされると、ファイルを選択するためのオープンパネルを表示しファイルを読み込むようにします。

![image](/images/ja/markdown-viewer/file_open.png)

[File]->[Open...] はボタンと同じように、アクションメソッドが必要となります。以下のようなメソッドを用意しておきます。

```ruby
  def open(sender)
    # [File]->[Open...] のアクション
  end
```

[Main Menu] の中にある [File]->[Open...] から、control キーを押しながら App Delegate までドラッグして、さきほど用意した `open` とアクションを接続します。

![image](/images/ja/markdown-viewer/connect_action.png)

これで、[File]->[Open...] がクリックされると `open` が呼び出されます。引き続き、以下のようなオープンパネルを表示するための処理を `open` へ実装します。

![image](/images/ja/markdown-viewer/open_panel.png)

オープンパネルの表示には [NSOpenPanel](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/ApplicationKit/Classes/NSOpenPanel_Class/Reference/Reference.html) を利用します。

```ruby
  def open(sender)
    # オープンパネルのインスタンスを取得
    panel = NSOpenPanel.openPanel

    # パネルを表示
    result = panel.runModalForDirectory(NSHomeDirectory(),                # パネルを表示するときのディレクトリ
                                        file: nil,                        # 選択できるファイル名を指定。指定しないので nil とする
                                        types: ["md", "mkd", "markdown"]) # 選択できるファイル拡張子を指定。
    if(result == NSOKButton)
      # パネルの OK がクリックされたときの処理
      path = panel.filename # 選択されたファイルのパスを取得

      # ファイルを読む
      markdown_content = File.read(path)
    end
  end
```


## Markdown を HTML へ変換し表示する
Markdown ファイルを読むまでの処理ができましたので、つぎに HTML へ変換する処理を実装します。まず、`rdiscount` を *AppDelegate.rb* の先頭で `require` でロードします。

```ruby
require 'rubygems'
require 'rdiscount'

class AppDelegate
  attr_accessor :window
  attr_accessor :webView
```

`open` メソッドでファイルを読んだ後の処理として、Markdown を HTML へ変換し表示する処理を実装します。

```ruby
      # ファイルを読む
      markdown_content = File.read(path)

      # Markdown を HTML へ変換
      md = RDiscount.new(markdown_content)

      # 変換された HTML にヘッダなどを追加
      html =<<"EOS"
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
</head>
<body>
#{md.to_html}
</body>
</html>
EOS
      # WebView に HTML を表示
      webView.mainFrame.loadHTMLString(html, baseURL:nil)
```

`webView.mainFrame` の `loadHTMLString` を用いると作成した HTML を表示できます。

![image](/images/ja/markdown-viewer/markdown-viewer.png)

スタイルシートを記述して見栄えをよくしたりしてみてください。Markdown ファイルで使用している画像を表示するために `baseURL` の指定を変更する必要があるかと思います。いろいろ試してみてください。その際には、MacRuby のサンプルにある [MarkdownViewer](https://github.com/MacRuby/MacRuby/tree/master/sample-macruby/MarkdownViewer) が参考になるかと思います。


## Deployment のオプションを追加
Deployment するときに、今回使用した rdiscount がアプリケーションに埋め込まれるように `--gem rdiscount` をオプションに追加しておきましょう。

![image](/images/ja/markdown-viewer/deployment_option.png)

<div class="note">
Deployment するとロードパスの探索範囲内に rdiscount が埋め込まれるようになります。そのため、Deployment する際に <code>require 'rubygems'</code> を削除しても動作します。(rubygems を読み込まない分、起動が速くなるかもしれません、という程度ですが。)
</div>

## 付録 : 今回作成したコード
最後に今回作成した *AppDelegate.rb* の全文を記載します。

```ruby
require 'rubygems'
require 'rdiscount'

class AppDelegate
  attr_accessor :window
  attr_accessor :webView

  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end

  def open(sender)
    # オープンパネルのインスタンスを取得
    panel = NSOpenPanel.openPanel

    # パネルを表示
    result = panel.runModalForDirectory(NSHomeDirectory(),                # パネルを表示するときのディレクトリ
                                        file: nil,                        # 選択できるファイル名を指定。指定しないので nil とする
                                        types: ["md", "mkd", "markdown"]) # 選択できるファイル拡張子を指定。
    if(result == NSOKButton)
      # パネルの OK がクリックされたときの処理
      path = panel.filename # 選択されたファイルのパスを取得

      # ファイルを読む
      markdown_content = File.read(path)

      # Markdown を HTML へ変換
      md = RDiscount.new(markdown_content)

      # 変換された HTML にヘッダなどを追加
      html =<<"EOS"
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
</head>
<body>
#{md.to_html}
</body>
</html>
EOS
      # WebView に HTML を表示
      webView.mainFrame.loadHTMLString(html, baseURL:nil)
    end
  end

end
```
