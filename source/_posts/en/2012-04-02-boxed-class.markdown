---
layout: post
title: "Boxed Class"
date: 2012-04-02 14:00
comments: true
sharing: true
categories: MacRuby
---

Boxed class is used to retrieve a structure information that is defined in Cocoa. All structures allow to retrieve an information by Boxed Class.


## Methods in Boxed Class
### Boxed.type
Returns a structure type information.

- type -> String
  - [RETURN]
	- Returns a structure type information.

```ruby
>> framework 'Cocoa'
>> NSRect.type
=> "{CGRect={CGPoint=dd}{CGSize=dd}}"
```

### Boxed.opaque?
Returns whether structure is opaque.

- opaque? -> bool
  - [RETURN]
	- Returns a true if structure is opaque. Otherwise, returns a false.

```ruby
>> framework 'Cocoa'
>> NSRect.opaque?
=> false
>> NSModalSession.opaque?
=> true
>> 
```
