---
layout: post
title: "ストップウォッチを作る (Thread 編)"
date: 2012-03-08 15:49
comments: true
sharing: true
categories: MacRuby
---

[ストップウォッチを作る](/blog/2012/03/07/intro-stopwatch/)では [NSTimer](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/nstimer_Class/Reference/NSTimer.html) を使って定期的に Text Field の内容を更新していました。同じ事を Ruby の Thread を使って実現することができます。

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # アウトレット
  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end
  
  def startTimer(sender)
    if @thread.nil?
      @time = 0.0
      @thread = Thread.start do
        loop do
          string = sprintf("%.1f", @time)
          textField.stringValue = string

          sleep 0.1
          @time += 0.1
        end
      end
    end
  end
  
  def stopTimer(sender)
    if @thread
      @thread.kill
      @thread = nil
    end
  end
end
```

[ストップウォッチを作る](/blog/2012/03/07/intro-stopwatch/)と同じように UI 部品を配置し、アウトレットとアクションメソッドを接続します。

NSTimer の長い呼び出しの記述が無くなっただけで、ずいぶん Ruby らしくなったのではないでしょうか。

また Text Field に文字列を設定するメソッドとして、今回は `stringValue=` を使用しています。MacRuby には `foo=` というメソッドを `setFoo` と読み替える仕組みがあり、それを活用するとより Ruby らしいコードになるのではないでしょうか。

<p class="note">
今回の Text Field のように、UI を更新する場合には単一のスレッドで行いましょう。(NSTimerを使ったものは、メインスレッドで更新しています)
</p>