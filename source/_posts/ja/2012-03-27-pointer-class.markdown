---
layout: post
title: "Pointer クラス"
date: 2012-03-27 12:00
comments: true
sharing: true
categories: MacRuby
---

Cocoa API を使用していると、引数にポインタ変数を渡さなければいけないときがあります。多くは `NSError* error;` のような変数が必要になるケースでしょうか。Ruby にはポインタ変数を扱うことができるクラスが存在しないため、MacRuby では Pointer クラスが追加されています。

Pointer クラスを使用して、Objective-C の `NSError* error;` と等しい変数を用意するには以下のような記述になります。

```ruby
error = Pointer.new('@')
```

`Pointer.new` の引数の `@` は、「オブジェクトのポインタ変数をつくりなさい」と指示しています。`@` のほかにも種類があり [Type Encodings](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html) で確認することができます。

`char* name[5];` のようなインスタンスを作成するには、以下のように `Pointer.new` の第 2 引数でサイズを指定します。

```ruby
name = Pointer.new('c', 5)
name[0] = 'a'
name[1] = 'b'
name[2] = 'c'
name[3] = 'd'
name[4] = 'e'
```

`NSRect *rect[2];` のような構造体のポインタ変数のインスタンスは、以下のように作成できます。

```ruby
rect = Pointer.new("{CGRect={CGPoint=dd}{CGSize=dd}}", 2)
```

構造体の内容を正確に把握しなければ、変数を用意することができません。
そこで、`NSRect.type` と構造体に対して `type` メソッドを実行すると `"{CGRect={CGPoint=dd}{CGSize=dd}}"` と構造体の Type を取得することができるようになっています。上の例は以下のように書くことができます。

```ruby
rect = Pointer.new(NSRect.type, 2)
```


## ポインタ種類の別名

`@` ではどのようなポインタとなるのかわかりにくいため、MacRuby では `Pointer.new('@')` を以下のように書くこともできます。

```ruby
error = Pointer.new(:object)
```

以下の表のように別名が用意されています。

<table class="table">
<tr><th>内容</th><th>ポインタ</th><th>別名</th>
<tr><td>char</td><td>Pointer.new('c')</td><td>Pointer.new(:char)</td></tr>
<tr><td>unsigned char</td><td>Pointer.new('C')</td><td>Pointer.new(:uchar)</td></tr>
<tr><td>short</td><td>Pointer.new('s')</td><td>Pointer.new(:short)</td></tr>
<tr><td>unsigned short</td><td>Pointer.new('S')</td><td>Pointer.new(:ushort)</td></tr>
<tr><td>int</td><td>Pointer.new('i')</td><td>Pointer.new(:int)</td></tr>
<tr><td>unsigned int</td><td>Pointer.new('I')</td><td>Pointer.new(:uint)</td></tr>
<tr><td>long</td><td>Pointer.new('l')</td><td>Pointer.new(:long)</td></tr>
<tr><td>unsigned long</td><td>Pointer.new('L')</td><td>Pointer.new(:ulong)</td></tr>
<tr><td>long long</td><td>Pointer.new('q')</td><td>Pointer.new(:long_long)</td></tr>
<tr><td>unsigned long long</td><td>Pointer.new('Q')</td><td>Pointer.new(:ulong_long)</td></tr>
<tr><td>float</td><td>Pointer.new('f')</td><td>Pointer.new(:float)</td></tr>
<tr><td>double</td><td>Pointer.new('d')</td><td>Pointer.new(:double)</td></tr>
<tr><td>character string (char *)</td><td>Pointer.new('*')</td><td>Pointer.new(:string)</td></tr>
<tr><td>pointer</td><td>Pointer.new('^')</td><td>Pointer.new(:pointer)</td></tr>
<tr><td>object</td><td>Pointer.new('@')</td><td>Pointer.new(:object)<br>Pointer.new(:id)</td></tr>
<tr><td>class object (Class)</td><td>Pointer.new('#')</td><td>Pointer.new(:class)</td></tr>
<tr><td>boolean</td><td>Pointer.new('B')</td><td>Pointer.new(:boolean)<br>Pointer.new(:bool)</td></tr>
<tr><td>method selector (SEL)</td><td>Pointer.new(':')</td><td>Pointer.new(:selector)<br>Pointer.new(:sel)</td></tr>
</table>


## Pointer クラスのメソッド

### Pointer.new
Pointer クラスのインスタンスを作成して返します。

- new(type, size = 1) -> Pointer
  - [PARAM] type:
	- どのようなポインタ変数を作成するか指定します。
  - [PARAM] size:
	- ポインタ変数のサイズを指定します。
  - [RETURN]
	- 作成した Pointer のインスタンスを返します。

### Pointer.new_with_type
Pointer.new の別名です。


### Pointer.magic_cookie
即値を (void *) へキャストした Pointer のインスタンスを返します。

- magic_cookie(val) -> Pointer
  - [PARAM] val:
	- 数値を指定します。
  - [RETURN]
	- (void *) に即値をキャストした Pointer のインスタンスを返します。

OpenGL に関するチケット [#1112](http://www.macruby.org/trac/ticket/1112) でこのメソッドが追加されました。


### Pointer#type
どのような種類のポインタか確認するのに使用します。

- type -> String
  - [RETURN]
	- ポインタの種類を返します。

```ruby
>> framework 'Cocoa'
>> pointer = Pointer.new(NSRect.type)
>> pointer.type
=> "{CGRect={CGPoint=dd}{CGSize=dd}}"
```

### Pointer#cast!
ポインタの種類を変更します。

- cast!(type) -> self
  - [PARAM] type:
	- 変更先のポインタの種類を指定します。
  - [RETURN]
	- ポインタの種類を変更したオブジェクトを返します。

```ruby
>> pointer = Pointer.new('i')
>> pointer.type
=> "i"
>> pointer.cast!('I')
>> pointer.type
=> "I"
```


### Pointer#[]
nth 番目の内容を取得します。

- self[nth]
  - [PARAM] nth:
	- 取得する内容の位置を指定します。
  - [RETURN]
	- nth 番目の内容を返します。


### Pointer#[]=
nth 番目の内容を val で置き換えます。

- self[nth] = val
  - [PARAM] nth:
	- 置き換える内容の位置を指定します。
  - [PARAM] val:
	- 置き換える内容を指定します。
  - [RETURN]
	- val を返します。


### Pointer#assign
0 番目の内容を val で置き換えます。

- assign(val)
  - [PARAM] val:
	- 置き換える内容を指定します。
  - [RETURN]
	- val を返します。


### Pointer#+
指定した offset からの Pointer インスタンスを返します。

- self + offset -> Pointer
  - [PARAM] offset:
	- 取得する内容の位置を指定します。
  - [RETURN]
	- 指定した位置からの Pointer インスタンスを返します。

```ruby
name = Pointer.new('c', 5)
name[0] = 10
name[1] = 11
name[2] = 12
name[3] = 13
name[4] = 14

tmp = name + 3
2.times do |i|
  p tmp[i] # => 13, 14
end
```

### Pointer#-
Pointer#+ と同じように、指定した offset からの Pointer インスタンスを返します。

- self - offset -> Pointer
  - [PARAM] offset:
	- 取得する内容の位置を指定します。
  - [RETURN]
	- 指定した位置からの Pointer インスタンスを返します。


### Pointer#to_object
TBD
