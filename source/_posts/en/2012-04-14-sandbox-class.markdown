---
layout: post
title: "Sandbox Class"
date: 2012-04-14 14:00
comments: true
sharing: true
categories: MacRuby
---

MacRuby has the Sandbox class which restricts the access to network. Sandbox class is implemented using the [sandbox(7)](https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man7/sandbox.7.html) in MacRuby.

You can restrict easily your application to access the network by `Sandbox.no_network.apply!`. Ruby methods and Cocoa APIs both are restricted by the Sandbox.

```ruby
>> framework 'Cocoa'
>> require 'socket'
>> Sandbox.no_network.apply!
>> Socket.gethostbyaddr("apple.com")
SocketError: host not found
	
>> NSHost.hostWithName("apple.com")
=> <NSHost 0x40121eb80> (null) ((
) (
))
```

The Sandbox is a good companion to the Ruby standard $SAFE functionality, you may use the Sandbox and $SAFE at the same time.

The Sandbox will be able to restrict your application,

- TCP/IP networking is prohibited.
- All sockets-based networking is prohibited. 
- File system writes are prohibited.
- File system writes are restricted to temporary folders.
- All operating system services are prohibited.

As notes,

- Restriction is not able to change after applying.
- Restricts with respect to each process.

## Methods in Sandbox Class
### Sandbox.no_internet

Restricts TCP/IP networking in current process.

- no_internet -> Sandbox
  - [RETURN]
	- Returns a Sandbox instance.


### Sandbox.no_network

Restricts all sockets-based networking in current process.

- no_network -> Sandbox
  - [RETURN]
	- Returns a Sandbox instance.


### Sandbox.no_writes

Restricts to write in current process.

- no_writes -> Sandbox
  - [RETURN]
	- Returns a Sandbox instance.


### Sandbox.temporary_writes

Restricts to write outside temporay folders in current process.

- temporary_writes -> Sandbox
  - [RETURN]
	- Returns a Sandbox instance.


### Sandbox.pure_computation

Restrictsn all operating system services in current process.

- pure_computation -> Sandbox
  - [RETURN]
	- Returns a Sandbox instance.


### Sandbox#apply!

Applies the restriction.

- apply!