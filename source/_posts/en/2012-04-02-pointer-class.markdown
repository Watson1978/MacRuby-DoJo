---
layout: post
title: "Pointer Class"
date: 2012-04-02 10:00
comments: true
sharing: true
categories: MacRuby
---

If you use the Cocoa APIs, you might have to pass a pointer variable into argument of API. Some cases, you might need a variable such as `NSError* error;`.

To create a pointer instance as `NSError* error;`, you can write a program as following.

```ruby
error = Pointer.new('@')
```

You can create other pointer instance if you pass a pointer type into `Pointer.new`. You can find the other pointer type [Type Encodings](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html).

To create a pointer instance such as `char* name[5];`, specify the size in the second argument.

```ruby
name = Pointer.new('c', 5)
name[0] = 'a'
name[1] = 'b'
name[2] = 'c'
name[3] = 'd'
name[4] = 'e'
```

To create a pointer instance of structure such as `NSRect *rect[2];`, you may write a program as following.

```ruby
rect = Pointer.new("{CGRect={CGPoint=dd}{CGSize=dd}}", 2)
```

Or,

```ruby
rect = Pointer.new(NSRect.type, 2)
```


## Alias of Pointer Types
You may think difficult the pointer types such as '@'. MacRuby has the alias of pointer types. 

```ruby
error = Pointer.new(:object)  # alias of '@'
```

<table class="table">
<tr><th>Meaning</th><th>Pointer Types</th><th>Alias</th>
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


## Methods of Pointer Class

### Pointer.new
Returns a new pointer instance.

- new(type, size = 1) -> Pointer
  - [PARAM] type:
	- Specifies a pointer type.
  - [PARAM] size:
	- Specifies a size.
  - [RETURN]
	- Returns a new pointer instance.


### Pointer.new_with_type
This method is alias of `Pointer.new`.


### Pointer.magic_cookie
Returns a new pointer instance which cast an immediate value to (void *).

- magic_cookie(val) -> Pointer
  - [PARAM] val:
	- Passes an immediate value to cast.
  - [RETURN]
	- Returns a new pointer instance.


### Pointer#type
Returns a pointer type.

- type -> String
  - [RETURN]
	- Returns a string as pointer type.

```ruby
>> framework 'Cocoa'
>> pointer = Pointer.new(NSRect.type)
>> pointer.type
=> "{CGRect={CGPoint=dd}{CGSize=dd}}"
```


### Pointer#cast!
Changes a pointer type.

- cast!(type) -> self
  - [PARAM] type:
	- Specifies a point type to change.
  - [RETURN]
	- Returns a self which pointer type was changed.

```ruby
>> pointer = Pointer.new('i')
>> pointer.type
=> "i"
>> pointer.cast!('I')
>> pointer.type
=> "I"
```


### Pointer#[]
Get a value at nth position.

- self[nth]
  - [PARAM] nth:
	- Specifies a position to get a value.
  - [RETURN]
	- Returns a value.


### Pointer#[]=
Set a value into nth position.

- self[nth] = val
  - [PARAM] nth:
	- Specifies a position to set a value.
  - [PARAM] val:
	- Specifies a value.
  - [RETURN]
	- Returns a `val`.


### Pointer#value
Get a value at 0 position.

- value
  - [RETURN]
    - Returns a value at 0 position.

```ruby
pointer = Pointer.new('c')
pointer[0] = 42

pointer[0]    # => 42
pointer.value # => 42
```


### Pointer#assign
Set a value into 0 position.

- assign(val)
  - [PARAM] val:
	- Specifies a position to set a value.
  - [RETURN]
	- Returns a `val`.


### Pointer#+
Returns a new pointer instance from the specified offset.

- self + offset -> Pointer
  - [PARAM] offset:
	- Specifies an offset.
  - [RETURN]
	- Returns a new pointer instance 

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
Returns a new pointer instance from the specified offset.

- self - offset -> Pointer
  - [PARAM] offset:
	- Specifies an offset.
  - [RETURN]
	- Returns a new pointer instance 


### Pointer#to_object
TBD