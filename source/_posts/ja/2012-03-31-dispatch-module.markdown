---
layout: post
title: "Dispatch モジュール"
date: 2012-03-31 10:00
comments: true
sharing: true
categories: MacRuby
---

Mac OS X 10.6 から追加された [Grand Central Dispatch (GCD)](https://developer.apple.com/library/mac/#documentation/Performance/Reference/GCD_libdispatch_Ref/Reference/reference.html) を利用するために、MacRuby には Dispatch モジュールと、以下のようにいくつかのクラスがあります。

- Dispatch モジュール
- [Dispatch::Queue クラス](/blog/2012/03/31/dispatch-queue-class/)
- [Dispatch::Group クラス](/blog/2012/03/31/dispatch-group-class/)
- [Dispatch::Source クラス](/blog/2012/03/31/dispatch-source-class/)
- [Dispatch::Semaphore クラス](/blog/2012/03/31/dispatch-semaphore-class/)

RubyGems の [Dispatch](https://github.com/gunn/Dispatch) ライブラリを用いると、より簡単に GCD を使うこともできます。


## Dispatch モジュールの定数
### Dispatch::TIME_NOW
「処理をすぐに開始する」と指示するときに使用します。GCD API で `DISPATCH_TIME_NOW` として定義されている定数と同じ定数です。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdq.after(Dispatch::TIME_NOW) { puts "Hello" }

sleep 0.5
```

### Dispatch::TIME_FOREVER
処理が完了するのをタイムアウトなしで待ち続けたりするときに使用します。GCD API で `DISPATCH_TIME_FOREVER` として定義されている定数と同じ定数です。

```ruby
gcdq = Dispatch::Queue.new('sample')
sema = Dispatch::Semaphore.new(0)
gcdq.async {
  puts "Hello, "
  sleep 1
  sema.signal
}

puts "Waiting..."

sema.wait(Dispatch::TIME_FOREVER)
puts "World"
```

## Dispatch モジュールのメソッド

### Dispatch#resume!
中断されている処理を再開します。GCD API の `dispatch_resume` に相当します。

- resume!

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdq.after(Dispatch::TIME_NOW) { 
  sleep 0.5
  puts "Hello"
}
gcdq.suspend!
gcdq.suspended?  #=> true
gcdq.resume!

sleep 1
```

### Dispatch#suspend!
処理を中断します。GCD API の `dispatch_suspend` に相当します。

- suspend!

### Dispatch#suspended?
処理が中断されているかを返します。

- suspended? -> bool
  - [RETURN]
    - 処理が中断されていれば true、そうでなければ false を返します。
