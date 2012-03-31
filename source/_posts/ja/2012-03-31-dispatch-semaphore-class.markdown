---
layout: post
title: "Dispatch::Semaphore クラス"
date: 2012-03-31 10:00
comments: true
sharing: true
categories: MacRuby
---

Dispatch::Semaphore クラスは排他制御機能を提供します。たとえば、Concurrent Dispatch Queue を用いて並行処理をしていた際に計算結果を 1 つの配列に格納する場合などに Dispatch Semaphore で排他制御を行います。

## Dispatch::Semaphore クラスのメソッド
### Dispatch::Semaphore.new
Dispatch Semaphore を生成し返します。GCD API の `dispatch_semaphore_create` に相当します。

- new(count) -> Semaphore
  - [PARAM] count:
 	  - セマフォのカウンタの初期値を指定します。
  - [RETURN]
    - 生成した Dispatch Semaphore のインスタンスを返します。

### Dispatch::Semaphore#wait
Dispatch::Semaphore#signal が呼び出されセマフォのカウンタが 1 以上になるまで待ちます。カウンタが 1 以上になると 1 減算して待ち状態が解除されます。GCD API の `dispatch_semaphore_wait` に相当します。

- wait(timeout = nil) -> bool
  - [PARAM] timeout:
	- タイムアウト時間を指定します。省略された場合には Dispatch::TIME_FOREVER が指定されたのと同じになります。
  - [RETURN]
    - Dispatch::Semaphore#signal で wait が解除された場合には true、タイムアウトした場合には false を返します。

```ruby
gcdq = Dispatch::Queue.new('sample')
sema = Dispatch::Semaphore.new(0)
gcdq.async {
  sleep 1
  puts "Hello"
  sema.signal
}
sema.wait #=> true
```

### Dispatch::Semaphore#signal
Dispatch Semaphore のカウンタを 1 加算します。GCD API の `dispatch_semaphore_signal` に相当します。

- signal -> bool
  - [RETURN]
    - セマフォを待っているスレッドがなければ true、それ以外で false を返します。
