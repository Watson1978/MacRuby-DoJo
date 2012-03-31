---
layout: post
title: "Dispatch::Group クラス"
date: 2012-03-31 10:00
comments: true
sharing: true
categories: MacRuby
---

Dispatch Queue に追加された複数の処理がすべて完了するまで待ちたい場合があるかと思います。そのような場合に Dispatch Group を用います。

### Dispatch::Group.new
Dispatch Group を生成します。

- new -> Group
  - [RETURN]
    - 生成した Dispatch Group のインスタンスを返します。

### Dispatch::Group#notify
Dispatch Queue のタスクがすべて完了すると実行する block を Dispatch Queue に追加します。GCD API の `dispatch_group_notify` に相当します。

- notify(queue) { ... }
  - [PARAM] queue:
	- block を追加する Dispatch Queue を指定します。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdg = Dispatch::Group.new
@i = 0
gcdq.async { @i = 42 }
gcdg.notify(gcdq) { p @i } # => 42
```

### Dispatch::Group#on_completion
Dispatch::Group#notify の別名です。

### Dispatch::Group#wait
Dispatch Group に所属しているDispatch Queue のタスクがすべて完了するのを待ちます。GCD API の `dispatch_group_wait` に相当します。

- wait(timeout = nil) -> bool
  - [PARAM] timeout:
	- タイムアウト時間を指定します。省略された場合には Dispatch::TIME_FOREVER が指定されたのと同じになります。
  - [RETURN]
    - すべてのタスクが完了していれば true、タイムアウトにより完了していないタスクがあれば false を返します。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdg = Dispatch::Group.new
gcdq.async(gcdg) { sleep 5 }
gcdg.wait # => true

gcdq.async(gcdg) { sleep 5 }
gcdg.wait(1) # => false
```
