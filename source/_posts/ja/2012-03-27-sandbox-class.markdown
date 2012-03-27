---
layout: post
title: "Sandbox クラス"
date: 2012-03-27 14:30
comments: true
sharing: true
categories: MacRuby
---

MacRuby には Sandbox というネットワークアクセスなどを制限するためのクラスがあります。[sandbox(7)](https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man7/sandbox.7.html) を利用して MacRuby に実装されています。

使い方は簡単で、あらかじめ `Sandbox.no_network.apply!` というように呼び出しておくだけです。Ruby のメソッド、Cocoa のAPI、双方に制限がかかります。

```
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

Sandbox を用いて、スクリプトに以下の制限をかけることができます。

- TCP/IPネットワーキング機能
- ソケットベースな全てのネットワーキング機能
- ファイルの書き込み
- /var/tmp など、テンポラリディレクトリ内のみファイル書き込み可能
- OS のサービス全て

注意点として、

- いったん制限をしてしまうと解除できない (別の制限に変更できない)
- Sandboxはプロセス単位で制限がかかる

制限をかけたい処理は別のプロセスで行うと良いでしょう。


## Sandbox クラスのメソッド

### Sandbox.no_internet
TCP/IPネットワーキング機能を制限します。

- no_internet -> Sandbox
  - [RETURN]
	- 制限内容が設定された、Sandbox のインスタンスを返します。


### Sandbox.no_network
ソケットベースな全てのネットワーキング機能を制限します。

- no_network -> Sandbox
  - [RETURN]
	- 制限内容が設定された、Sandbox のインスタンスを返します。


### Sandbox.no_writes
ファイルの書き込みを制限します。

- no_writes -> Sandbox
  - [RETURN]
	- 制限内容が設定された、Sandbox のインスタンスを返します。


### Sandbox.temporary_writes
/var/tmp など、テンポラリディレクトリ内のみファイル書き込み可能にします。

- temporary_writes -> Sandbox
  - [RETURN]
	- 制限内容が設定された、Sandbox のインスタンスを返します。


### Sandbox.pure_computation
OS のサービス全てを制限します。

- pure_computation -> Sandbox
  - [RETURN]
	- 制限内容が設定された、Sandbox のインスタンスを返します。


### Sandbox#apply!
制限内容を反映します。

- apply!
