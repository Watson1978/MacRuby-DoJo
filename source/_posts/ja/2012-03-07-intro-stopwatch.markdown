---
layout: post
title: "ストップウォッチを作る"
date: 2012-03-07 00:44
comments: true
sharing: true
categories: MacRuby
---

ここでは、簡易的な「ストップウォッチ」アプリケーションを作成します。 単純なアプリケーションですが、MacRuby でアプリケーションを作成するうえで不可欠な内容となっています。

アプリケーションは以下の図のようなデザインで、「StopWatch」というプロジェクトとします。

![image](/images/ja/intro-stopwatch/stopwatch.png)

「start ボタンがクリックされるとタイマーが動き出し、stop ボタンがクリックされるとタイマーを停止します。Text Field には何秒間タイマーが動いていたかを表示する。」というアプリケーションです。

## UI 部品を配置する
それでは、さっそく UI 部品を配置してみましょう。Xcode で *MainMenu.xib* ファイルを選択して、UI デザインを変更していきます。

![image](/images/ja/intro-stopwatch/mainmenu_xib.png)

次に [Window - StopWatch] を選択して、UI 部品を配置する Window を表示します。

![image](/images/ja/intro-stopwatch/window_stopwatch.png)

以下の図のように、ツールバーのアイコンをクリックすると Object Library が表示されます。

![image](/images/ja/intro-stopwatch/show_object_library.png)

ようやく UI 部品を配置するための準備が整いました。

それでは、タイマーの値を表示するための TextField と、タイマーの開始と停止のための Button を Object Library から Window へドラッグ&ドロップして配置します。

![image](/images/ja/intro-stopwatch/ui_design.png)


## アウトレットを接続する
「ストップウォッチ」アプリケーションではタイマーの値を Text Field へ設定しますが、Text Field を Window に配置しただけでは値を設定することができません。UI 部品の状態や値を設定するための仕組みとしてアウトレットがあります。

アウトレットを使用するために、プログラムへ記述が必要となります。*AppDelegate.rb* を選択して以下のように `attr_accessor :textField` を AppDelegate クラスに追加します。

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # アウトレット
```

このように attr_accessor で用意した変数をアウトレットとして使用することができます。

*MainMenu.xib* の画面に戻り、アウトレットを接続します。<kbd>control</kbd> キーを押しながら、App Delegate から Text Field へドラッグします。

![image](/images/ja/intro-stopwatch/connect_outlet.png)

アウトレットの一覧が表示されるので、先ほど追加した textField を選択して接続します。

![image](/images/ja/intro-stopwatch/outlets.png)

これで、プログラムから textField 変数を通して Text Field の値を取得設定できるようになります。


## アクションを接続する
まだ、start ボタンと stop ボダンをクリックしても何も起こりません。ボタンがクリックされた時の振る舞いを設定してあげる必要があります。振る舞いを設定する仕組みとしてアクションがあります。

*AppDelegate.rb* を選択して以下のように `startTimer` と `stopTimer` を追加します。

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # アウトレット
  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end
  
  def startTimer(sender)
    # タイマー開始
  end
  
  def stopTimer(sender)
    # タイマー停止
  end
  
end
```

*MainMenu.xib* の画面に戻り、アクションを接続します。<kbd>control</kbd> キーを押しながら、start ボタンから App Delegate へドラッグします。

![image](/images/ja/intro-stopwatch/connect_action.png)

アクションの一覧が表示されるので、先ほど追加した `startTimer` を選択して接続します。

![image](/images/ja/intro-stopwatch/actions.png)

同じように stop ボタンと `stopTimer` を接続します。これで、ボタンがクリックされた時にそれぞれのメソッドが呼び出されるようになります。

ボタンがクリックされた時に呼び出される `startTimer` と `stopTimer` は**アクションメソッド**とも呼ばれます。

<p class="note">
アクションメソッドを記述する際には、必ず sender という引数を用意しましょう。MacRuby の仕様により、sender 引数がないメソッドはアクションメソッドとして認識されず、接続することができません。
</p>


## タイマーを使う
一定間隔ごとに何か処理を行う場合には、[NSTimer](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/nstimer_Class/Reference/NSTimer.html) を使用します。

以下のようにタイマーをすると、一定間隔ごとに処理することができます。

```ruby
@timer = NSTimer
           .scheduledTimerWithTimeInterval(0.1,
                                           target: self,
                                           selector: "timerHandler:",
                                           userInfo: nil,
                                           repeats: true)

def timerHandler(obj)
  # 一定間隔ごとに処理する内容
end

```

<table class="table">
<tr><th>引数</th><th>内容</th></tr>
<tr><td></td><td>タイマーを発生させる間隔(上の例では0.1秒)</td></tr>
<tr><td>target</td><td>タイマー発生時に呼び出すメソッドが存在するターゲット</td></tr>
<tr><td>selector</td><td>タイマー発生時に呼び出すメソッド</td></tr>
<tr><td>userInfo</td><td>メソッドに渡すデータ</td></tr>
<tr><td>repeats</td><td>タイマーの実行を繰り返すかどうか (true:繰り返す, false:1度のみ)</td></tr>
</table>

<p class="note">
selector はメッソド名を示す文字列です。メソッドが引数を取る場合には "timerHandler:" のようにコロンがついたりします。引数がない場合には "timerHandler" です。 上記の例で使用しているメソッドの場合には "scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:" となります。Objective-C 由来の書き方でわかりにくいのですが、リファレンスを読む場合にも度々でてくるので、selector の書き方はマスターしてください。
</p>

タイマーを停止するには、`invalidate` メソッドを使用します。

```ruby
@timer.invalidate
```

それではアクションメソッドの `startTimer` でタイマーを生成し、`stopTimer` で停止するようにしましょう。

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # アウトレット
  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end
  
  def startTimer(sender)
    if @timer.nil?
      @time = 0.0
      @timer = NSTimer
                .scheduledTimerWithTimeInterval(0.1,
                                                target: self,
                                                selector: "timerHandler:",
                                                userInfo: nil,
                                                repeats: true)
    end
  end
  
  def stopTimer(sender)
    if @timer
      @timer.invalidate
      @timer = nil
    end
  end
  
  def timerHandler(obj)
    @time += 0.1
    string = sprintf("%.1f", @time)
    textField.setStringValue(string) 
  end
end
```

`timerHandler` で Text Field に経過秒数を設定しています。`setStringValue` メソッドを使用すると、Text Field に文字列を設定できます。

これで「ストップウォッチ」アプリケーションが完成です。完成したアプリケーションを実行してみましょう。[Scheme] で「Deployment」からプロジェクト名のスキームに変更してツールバーの [Run] ボタンをクリックすると、アプリケーションが起動します。

![image](/images/ja/intro-stopwatch/scheme.png)