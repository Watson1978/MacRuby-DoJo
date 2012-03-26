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

```
>> "hello".transform("Upper")
=> "HELLO"
>> "HELLO".transform("Lower")
=> "hello"
>> "hello".transform("Hiragana")
=> "へっろ"
>> "hello".transform("Greek")
=> "ἑλλο"
```


### String#to_data
文字列から [NSData](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSData_Class/Reference/Reference.html) クラスのオブジェクトを生成し返します。

- to_data -> NSData
  - [RETURN]
	- NSData オブジェクトを返します。

```
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

```
>> data = "foo".to_data
=> <666f6f>
>> data.to_str
=> "foo"
```


### String#pointer
文字列から Pointer クラスのオブジェクトを返します。オブジェクトは `unsigned char*` の変数に対応します。Cocoa API では String オブジェクトではなく、文字列のポインタ変数を受け付けるメソッドがあります。それらのメソッドに渡すためのデータを用意するために使用します。

- pinter -> Pointer
  - [RETURN]
	- Pointer オブジェクトを返します。

```
>> pointer = "foo".pointer
=> #<Pointer:0x4007ac580>
>> data = NSData.dataWithBytes(pointer, length: "foo".length)
=> <666f6f>
>> data.to_str
=> "foo"
```


### Kernel.framework
フレームワークをロードします。

- framework -> bool
  - [RETURN]
	- フレームワークのロードに成功すれば true、失敗した場合には false を返します。

```
>> framework 'Foundation'
=> true
```


### Kernel.load_bridge_support_file
Bridge Support ファイルをロードします。

- load_bridge_support_file(filename) -> self

```
>> load_bridge_support_file("Foo.bridgesupport")
=> main
```


### Kernel.load_plist
plist 形式の文字列から Hash オブジェクトに変換し返します。

- load_plist(string) -> Hash
  - [PARAM] string:
	- plist データの文字列を渡します。
  - [RETURN]
	- plist データを Hash に変換したオブジェクトを返します。

```
>> data = File.read('StopWatch-Info.plist')
>> load_plist(data)
=> {"CFBundleName"=>"${PRODUCT_NAME}", "CFBundleIdentifier"=>"Watson.${PRODUCT_NAME:rfc1034identifier}", "CFBundleInfoDictionaryVersion"=>"6.0", "CFBundleVersion"=>"1", "CFBundleExecutable"=>"${EXECUTABLE_NAME}", "NSPrincipalClass"=>"NSApplication", "CFBundlePackageType"=>"APPL", "CFBundleIconFile"=>"MacRuby.icns", "CFBundleSignature"=>"????", "NSMainNibFile"=>"MainMenu", "LSMinimumSystemVersion"=>"${MACOSX_DEPLOYMENT_TARGET}", "CFBundleDevelopmentRegion"=>"en", "CFBundleShortVersionString"=>"1.0"}
```


### Object#to_plist
plist 形式に変換したデータを文字列で返します。

- to_plist -> String
  - [RETURN]
	- 文字列を返します。

```
>> "foo".to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<string>foo</string>\n</plist>\n"
>> {"foo" => 42}.to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<dict>\n\t<key>foo</key>\n\t<integer>42</integer>\n</dict>\n</plist>\n"
>> ["foo", "bar"].to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<array>\n\t<string>foo</string>\n\t<string>bar</string>\n</array>\n</plist>\n"
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
