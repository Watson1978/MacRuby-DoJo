---
layout: post
title: "Boxed クラス"
date: 2012-03-27 14:00
comments: true
sharing: true
categories: MacRuby
---

Boxed クラスは Cocoa で定義されている構造体の情報を取得するために使用します。すべての構造体で Boxed クラスのメソッドを使用できます。


## Boxed クラスのメソッド
### Boxed.type
構造体の型情報を返します。

- type -> String
  - [RETURN]
	- 構造体の型情報を返します。

```ruby
>> framework 'Cocoa'
>> NSRect.type
=> "{CGRect={CGPoint=dd}{CGSize=dd}}"
```

### Boxed.opaque?
構造体が Opaque 構造体かどうかを返します。Opaque 構造体については、[「ダイナミックObjective-C」](http://news.mynavi.jp/column/objc/018/)に書かれていますので参考にしてください。

- opaque? -> bool
  - [RETURN]
	- Opaque 構造体の場合には true、異なる場合には false を返します。

```ruby
>> framework 'Cocoa'
>> NSRect.opaque?
=> false
>> NSModalSession.opaque?
=> true
>> 
```
