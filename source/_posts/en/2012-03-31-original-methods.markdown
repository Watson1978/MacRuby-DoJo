---
layout: post
title: "Original Methods in MacRuby"
date: 2012-03-31 16:00
comments: true
sharing: true
categories: MacRuby
---

MacRuby has some original methods. They does not exist in CRuby's methods and Cocoa APIs. This content describes those methods.


## Original Methods
### String#transform
Transforms the string to uppercase/lowercase or another language characters.
This method is implemented with [ICU](http://site.icu-project.org/)  [Transforms](http://userguide.icu-project.org/transforms/general).

- transform(pattern) -> String
  - [PARAM] pattern:
	- Specifies transliteration identifier.
  - [RETURN]
	- Returns the transformed string.

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
Returns a Pointer object converted from String. Returned object corresponds to a variable of `unsigned char *`.

Cocoa APIs has some methods which accepts the pointer variable.
`String#pointer` may be used for these methods.

- pinter -> Pointer
  - [RETURN]
	- Returns a Pointer object converted from String.

```ruby
>> pointer = "foo".pointer
=> #<Pointer:0x4007ac580>
>> data = NSData.dataWithBytes(pointer, length: "foo".length)
=> <666f6f>
>> data.to_str
=> "foo"
```


### String#to_data
Returns an [NSData](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSData_Class/Reference/Reference.html) object which is converted from String.

- to_data -> NSData
  - [RETURN]
	- Returns a NSData object.

```ruby
>> "foo".to_data
=> <666f6f>
>> "foo".to_data.class
=> __NSCFData
```


### NSData#to_str
Returns a String object which is converted from NSData.

- to_str -> String
  - [RETURN]
	- Returns a String object

```ruby
>> data = "foo".to_data
=> <666f6f>
>> data.to_str
=> "foo"
```


### Kernel.framework
Loads a framework.

- framework -> bool
  - [RETURN]
	- Returns a true if loaded a framework. Otherwise, returns a false.

```ruby
>> framework 'Foundation'
=> true
```


### Kernel.load_bridge_support_file
Loads a Bridge Support file.

- load_bridge_support_file(filename) -> self

```ruby
>> load_bridge_support_file("Foo.bridgesupport")
=> main
```


### Kernel.load_plist
Returns an object which is converted from string of plist format.

- load_plist(string) -> Object
  - [PARAM] string:
	- Passes a string of plist format.
  - [RETURN]
	- Returns an object.

```ruby
>> data = File.read('StopWatch-Info.plist')
>> load_plist(data)
=> {"CFBundleName"=>"${PRODUCT_NAME}", "CFBundleIdentifier"=>"Watson.${PRODUCT_NAME:rfc1034identifier}", "CFBundleInfoDictionaryVersion"=>"6.0", "CFBundleVersion"=>"1", "CFBundleExecutable"=>"${EXECUTABLE_NAME}", "NSPrincipalClass"=>"NSApplication", "CFBundlePackageType"=>"APPL", "CFBundleIconFile"=>"MacRuby.icns", "CFBundleSignature"=>"????", "NSMainNibFile"=>"MainMenu", "LSMinimumSystemVersion"=>"${MACOSX_DEPLOYMENT_TARGET}", "CFBundleDevelopmentRegion"=>"en", "CFBundleShortVersionString"=>"1.0"}

>> data = "foo".to_plist
>> load_plist(data)
=> "foo"
```


### Object#to_plist
Returns a string of plist format which is converted from Object.

- to_plist -> String
  - [RETURN]
	- Returns a string of plist format.

```ruby
>> "foo".to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<string>foo</string>\n</plist>\n"
>> {"foo" => 42}.to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<dict>\n\t<key>foo</key>\n\t<integer>42</integer>\n</dict>\n</plist>\n"
>> ["foo", "bar"].to_plist
=> "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE plist PUBLIC \"-//Apple//DTD PLIST 1.0//EN\" \"http://www.apple.com/DTDs/PropertyList-1.0.dtd\">\n<plist version=\"1.0\">\n<array>\n\t<string>foo</string>\n\t<string>bar</string>\n</array>\n</plist>\n"
```


### Object#methods
Returns a methods list. CRuby also has `Object#methods`, `objc_methods` argument is added to MacRuby's method.

- methods(include_inherited = true, objc_methods = false) -> [Symbol]
  - [PARAM] include_inherited:
	- If passes a true, looks up a recursively.
  - [PARAM] objc_methods:
	- If passes a true, returns the Ruby's methods and Cocoa APIs.
  - [RETURN]
	- Returns a method list.

```ruby
> "foo".methods(true, true)
=> [:encode!, :"replaceCharactersInRange:withString:", :"getCharacters:range:", :characterAtIndex, :length, :transform, :crypt, :rpartition, :partition, :sum, :tr_s!, :tr_s, :tr!, :tr, :squeeze!, :squeeze, :delete!, :delete, :count, :reverse!, :reverse, :upto, :next!, :next, :succ!, :succ, :each_codepoint, :codepoints, :each_byte, :bytes, :each_char, :chars, :each_line, :lines, :rstrip!, :lstrip!, :strip!, :rstrip, :lstrip, :strip, :center, :rjust, :ljust, :capitalize!, :capitalize, :swapcase!, :swapcase, :upcase!, :upcase, :downcase!, :downcase, :gsub!, :gsub, :sub!, :sub, :chop!, :chop, :chomp!, :chomp, :to_f, :chr, :ord, :oct, :hex, :to_i, :split, :scan, :=~, :match, :dump, :inspect, :intern, :to_sym, :to_str, :to_s, :end_with?, :start_with?, :include?, :eql?, :casecmp, :<=>, :==, :concat, :<<, :%, :*, :+, :rindex, :index, :insert, :slice!, :slice, :[]=, :[], :ascii_only?, :valid_encoding?, :force_encoding, :pointer, :to_data, :setbyte, :getbyte, :bytesize, :empty?, :size, :encoding, :clear, :replace, :dup, :"performSelector:withObject:withObject:", :"performSelector:withObject:", :performSelector, :conformsToProtocol, :dd_appendSpaces, :"replaceOccurrencesOfString:withString:options:range:", :initWithCapacity,  ---- snip ----
```


### Range#relative_to
Returns a new Range instance which has negative values in `rng` expanded relative to `max`.

- relative_to(max) -> Range
  - [PARAM] max:
	- Specifies a max of Range.
  - [RETURN]
	- Returns a new Range instance.

```ruby
>> (1..10).relative_to(5)
=> 1..5
>> (-2..-1).relative_to(5)
=> 3..4
```


### BasicSocket#sendfile
Sends a file with socket. This method is implemented with [sendfile(2)](http://developer.apple.com/library/ios/#DOCUMENTATION/System/Conceptual/ManPages_iPhoneOS/man2/sendfile.2.html).

- sendfile(file, offset, length) -> Integer
  - [PARAM] file:
	- Specifies a file path or readable IO object.
  - [PARAM] offset:
	- Specifies a offset as start position.
  - [PARAM] length:
	- Specifies a length to send.
  - [RETURN]
	- Returns a sent length.

```ruby
require 'socket'

port = 5000
socket = TCPSocket.open('localhost', port)

path = 'sample.txt'
socket.sendfile(path, 0, 128)
```


## Fixnum#popcnt
Returns the number of 1 bits set in the internal representation of Fixnum.

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
