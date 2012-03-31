---
layout: post
title: "Dispatch::Queue クラス"
date: 2012-03-31 10:00
comments: true
sharing: true
categories: MacRuby
---

GCD を用いると簡単にマルチスレッドで処理を行うことができます。私たちは、GCD を使いたい処理を書き、適切な Dispatch Queue へ追加するだけです。あとは、GCD が必要なスレッドを生成して処理してくれます。

Dispatch Queue の種類として、以下の 4 つがあります。

- Serial Dispatch Queue
- Concurrent Dispatch Queue
- Main Dispatch Queue (Serial Dispatch Queue)
- Global Dispatch Queue (Concurrent Dispatch Queue)

Serial Dispatch Queue はタスクを逐次的に実行する際に用い、Concurrent Dispatch Queue は平行して複数の処理を行う際に用います。

Main Dispatch Queue と Global Dispatch Queue はあらかじめ用意されており、`new` でインスタンスを生成しなくとも使うことができます。Main Dispatch Queue は Serial Dispatch Queue の、Global Dispatch Queue は Concurrent Dispatch Queue の一種となっています。

## Dispatch::Queue クラスのメソッド

### Dispatch::Queue.new
逐次処理用に Serial Dispatch Queue を生成します。

- new(label) -> Queue
  - [PARAM] label:
 	  - 生成する Queue に付与するユニークなラベルを指定します。
  - [RETURN]
    - 生成した Dispatch Queue のインスタンスを返します。

### Dispatch::Queue.concurrent
並行処理用に Global Dispatch Queue を返します。Mac OS X 10.7 でビルドされた MacRuby では、優先度を指定すると Concurrent Dispatch Queue を返します。

- concurrent(priority = :default) -> Queue
  - [PARAM] priority:
    - 生成する Queue の優先度を指定します。`:high`、`:low`、`:default` と Mac OS X 10.7 からは  `:background` も指定できます。
  - [RETURN]
    - Dispatch Queue のインスタンスを返します。

### Dispatch::Queue.main
Main Dispatch Queue を返します。

- main -> Queue
  - [RETURN]
    - Main Dispatch Queue のインスタンスを返します。

### Dispatch::Queue.current
現在使用している Dispatch Queue を返します。GCD タスクの block 内で呼び出すと、そのタスクを実行している Queue が返ります。block の外側で呼び出すと Main Dispatch Queue が返ります。

- current -> Queue
  - [RETURN]
    - 現在使用している Dispatch Queue のインスタンスを返します。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdq.after(Dispatch::TIME_NOW) {
  q = Dispatch::Queue.current
  puts q.label # => sample
}

sleep 1

q = Dispatch::Queue.current
puts q.label # => com.apple.main-thread
```

### Dispatch::Queue#apply
指定した回数 block を Dispatch Queue に追加し、それらの処理がすべて完了するまで待ちます。GCD API の `dispatch_apply` に相当します。

- apply(iterations) {|index| ... }
  - [PARAM] iterations:
    - Dispatch Queue に block を追加する回数を指定します。

```ruby
gcdq = Dispatch::Queue.new('sample')
@result = []
gcdq.apply(5) {|i| @result[i] = i*i }
p @result  #=> [0, 1, 4, 9, 16, 25]
```

### Dispatch::Queue#async
長時間にわたる処理など、非同期的に実行したい block を Dispatch Queue に追加します。group を指定すると GCD API の `dispatch_group_async` に、それ以外は `dispatch_async` に相当します。

- async(group = nil) { ... }
  - [PARAM] group:
    - group を指定することで、Dispatch Queue に追加するタスクが group と関連づけられます。

```ruby
gcdq = Dispatch::Queue.new('sample')
@i = 0
gcdq.async { @i = 42 }
while @i == 0 do; end
p @i #=> 42
```

group を用いることで、Dispatch Queue に追加したすべてのタスクが完了するのを待ち合わせることができたりします。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdg = Dispatch::Group.new
@i = 0
gcdq.async(gcdg) { @i = 42 }
gcdg.wait
p @i #=> 42
```

### Dispatch::Queue#sync
同期的に実行したい block を Dispatch Queue に追加します。処理が完了するまで待ちます。GCD API の `dispatch_sync` に相当します。

- sync { ... }

```ruby
gcdq = Dispatch::Queue.new('sample')
@i = 0
gcdq.sync { @i = 42 }
p @i #=> 42
```

### Dispatch::Queue#after
指定した時間が経過した後、block を Dispatch Queue に追加します。

- after(delay) { ... }
  - [PARAM] delay:
    - 何秒後に block を Dispatch Queue に追加するか指定します。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdq.after(0.5) { puts "Hello" }

sleep 1
```

### Dispatch::Queue#label
Dispatch Queue のラベルを返します。GCD API の `dispatch_queue_get_label` に相当します。

- label -> String
  - [RETURN]
    - Dispatch Queue のラベルを返します。

```ruby
gcdq = Dispatch::Queue.new('sample')
gcdq.label # => 'sample'
```

### Dispatch::Queue#to_s
Dispatch::Queue#label の別名です。


### Dispatch::Queue#barrier_async
TBD

### Dispatch::Queue#barrier_sync
TBD

### Dispatch::Queue.main.run
TBD


