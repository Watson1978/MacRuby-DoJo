---
layout: post
title: "MacRuby 独自のメソッド"
date: 2012-03-26 12:00
comments: true
sharing: true
categories: MacRuby
---

MacRuby には CRuby や Cocoa API に存在しない、独自のメソッドやクラスが存在します。ここでは、独自メソッドについて説明します。


## 独自メソッド
### String#transform
文字列を大文字や小文字、または別の言語の文字列に変形することができます。[ICU](http://site.icu-project.org/) の [Transforms](http://userguide.icu-project.org/transforms/general) という機能を用いて実装されています。

- transform(pattern) -> String
  - [PARAM] pattern:
	- どのような文字列に変換するかを指定します。
  - [RETURN]
	- 変換結果の文字列を返します。

```ruby
>> "hello".transform("Upper")
=> "HELLO"
>> "HELLO".transform("Lower")
=> "hello"
>> "hello".transform("Hiragana")
=> "へっろ"
>> "hello".transform("Greek")
=> "ἑλλο"
```


### String#pointer
文字列から Pointer クラスのオブジェクトを返します。オブジェクトは `unsigned char*` の変数に対応します。Cocoa API では String オブジェクトではなく、文字列のポインタ変数を受け付けるメソッドがあります。それらのメソッドに渡すためのデータを用意するために使用します。

- pinter -> Pointer
  - [RETURN]
	- Pointer オブジェクトを返します。

```ruby
>> pointer = "foo".pointer
=> #<Pointer:0x4007ac580>
>> data = NSData.dataWithBytes(pointer, length: "foo".length)
=> <666f6f>
>> data.to_str
=> "foo"
```


### String#to_data
文字列から [NSData](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSData_Class/Reference/Reference.html) クラスのオブジェクトを生成し返します。

- to_data -> NSData
  - [RETURN]
	- NSData オブジェクトを返します。

```ruby
>> "foo".to_data
=> <666f6f>
>> "foo".to_data.class
=> __NSCFData
```


### NSData#to_str
文字列を伴う NSData オブジェクトから、文字列を返します。

- to_str -> String
  - [RETURN]
	- 文字列を返します。

```ruby
>> data = "foo".to_data
=> <666f6f>
>> data.to_str
=> "foo"
```


### Kernel.framework
フレームワークをロードします。

- framework -> bool
  - [RETURN]
	- フレームワークのロードに成功すれば true、失敗した場合には false を返します。

```ruby
>> framework 'Foundation'
=> true
```


### Kernel.load_bridge_support_file
Bridge Support ファイルをロードします。

- load_bridge_support_file(filename) -> self

```ruby
>> load_bridge_support_file("Foo.bridgesupport")
=> main
```


### Kernel.load_plist
plist 形式の文字列を Ruby オブジェクトに変換し返します。

- load_plist(string) -> Object
  - [PARAM] string:
	- plist データの文字列を渡します。
  - [RETURN]
	- オブジェクトを返します。

```ruby
>> data = File.read('StopWatch-Info.plist')
>> load_plist(data)
=> {"CFBundleName"=>"${PRODUCT_NAME}", "CFBundleIdentifier"=>"Watson.${PRODUCT_NAME:rfc1034identifier}", "CFBundleInfoDictionaryVersion"=>"6.0", "CFBundleVersion"=>"1", "CFBundleExecutable"=>"${EXECUTABLE_NAME}", "NSPrincipalClass"=>"NSApplication", "CFBundlePackageType"=>"APPL", "CFBundleIconFile"=>"MacRuby.icns", "CFBundleSignature"=>"????", "NSMainNibFile"=>"MainMenu", "LSMinimumSystemVersion"=>"${MACOSX_DEPLOYMENT_TARGET}", "CFBundleDevelopmentRegion"=>"en", "CFBundleShortVersionString"=>"1.0"}

>> data = "foo".to_plist
>> load_plist(data)
=> "foo"
```


### Object#to_plist
plist 形式に変換したデータを文字列で返します。

- to_plist -> String
  - [RETURN]
	- 文字列を返します。

```ruby
>> "foo".to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<string>foo</string>\n</plist>\n"
>> {"foo" => 42}.to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<dict>\n\t<key>foo</key>\n\t<integer>42</integer>\n</dict>\n</plist>\n"
>> ["foo", "bar"].to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<array>\n\t<string>foo</string>\n\t<string>bar</string>\n</array>\n</plist>\n"
```


### Object#methods
オブジェクトに対して呼び出せるメソッド名の一覧を返します。CRuby も Object#methods がありますが、MacRuby では `objc_methods` 引数が追加されています。

- methods(include_inherited = true, objc_methods = false) -> [Symbol]
  - [PARAM] include_inherited:
	- 引数が false の時は Object#singleton_methods(false) と同じになります。
  - [PARAM] objc_methods:
	- 引数が true の時は Ruby のメソッドのほかに Cocoa API の一覧も返します。
  - [RETURN]
	- メソッド名の一覧を返します。

```ruby
> "foo".methods(true, true)
=> [:encode!, :"replaceCharactersInRange:withString:", :"getCharacters:range:", :characterAtIndex, :length, :transform, :crypt, :rpartition, :partition, :sum, :tr_s!, :tr_s, :tr!, :tr, :squeeze!, :squeeze, :delete!, :delete, :count, :reverse!, :reverse, :upto, :next!, :next, :succ!, :succ, :each_codepoint, :codepoints, :each_byte, :bytes, :each_char, :chars, :each_line, :lines, :rstrip!, :lstrip!, :strip!, :rstrip, :lstrip, :strip, :center, :rjust, :ljust, :capitalize!, :capitalize, :swapcase!, :swapcase, :upcase!, :upcase, :downcase!, :downcase, :gsub!, :gsub, :sub!, :sub, :chop!, :chop, :chomp!, :chomp, :to_f, :chr, :ord, :oct, :hex, :to_i, :split, :scan, :=~, :match, :dump, :inspect, :intern, :to_sym, :to_str, :to_s, :end_with?, :start_with?, :include?, :eql?, :casecmp, :<=>, :==, :concat, :<<, :%, :*, :+, :rindex, :index, :insert, :slice!, :slice, :[]=, :[], :ascii_only?, :valid_encoding?, :force_encoding, :pointer, :to_data, :setbyte, :getbyte, :bytesize, :empty?, :size, :encoding, :clear, :replace, :dup, :"performSelector:withObject:withObject:", :"performSelector:withObject:", :performSelector, :conformsToProtocol, :dd_appendSpaces, :"replaceOccurrencesOfString:withString:options:range:", :initWithCapacity, 以下略
```


### Range#relative_to
`max` で指定した Range 範囲のオブジェクトを返します。負の範囲に対して実行した場合には、`max` との相対的な範囲となります。

- relative_to(max) -> Range
  - [PARAM] max:
	- Range の範囲の最大値を指定します。
  - [RETURN]
	- 新しい Range オブジェクトを返します。

```ruby
>> (1..10).relative_to(5)
=> 1..5
>> (-2..-1).relative_to(5)
=> 3..4
```


### BasicSocket#sendfile
ソケットを介してファイルのデータを [sendfile(2)](http://developer.apple.com/library/ios/#DOCUMENTATION/System/Conceptual/ManPages_iPhoneOS/man2/sendfile.2.html) を用いて送信します。

- sendfile(file, offset, length) -> Integer
  - [PARAM] file:
	- 送信するファイルのパス、または IO オブジェクトを渡します。
  - [PARAM] offset:
	- 送信を開始するデータの位置を指定します。
  - [PARAM] length:
	- 送信するデータサイズを指定します。
  - [RETURN]
	- 送信したデータサイズを返します。

```ruby
require 'socket'

port = 5000
socket = TCPSocket.open('localhost', port)

path = 'sample.txt'
socket.sendfile(path, 0, 128)
```


### Fixnum#popcnt
数値を 2 進数表記した際に、1 となるビットの数を返します。

- popcnt -> Fixnum

```ruby
>> 13.to_s(2)
=> "1101"
>> 13.popcnt
=> 3
>> 255.to_s(2)
=> "11111111"
>> 255.popcnt
=> 8
```
