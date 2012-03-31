---
layout: post
title: "Dispatch::Source クラス"
date: 2012-03-31 10:00
comments: true
sharing: true
categories: MacRuby
---

Dispatch::Source クラスは何らかのイベントが発生するときに、イベントをハンドリングして処理を実行する仕組みを提供します。

## Dispatch::Source クラスの定数
- Dispatch::Source::DATA_ADD
- Dispatch::Source::DATA_OR
- Dispatch::Source::PROC
- Dispatch::Source::SIGNAL
- Dispatch::Source::READ
- Dispatch::Source::WRITE
- Dispatch::Source::VNODE
- <br/>
- Dispatch::Source::PROC_EXIT
- Dispatch::Source::PROC_FORK
- Dispatch::Source::PROC_EXEC
- Dispatch::Source::PROC_SIGNAL
- <br/>
- Dispatch::Source::VNODE_DELETE
- Dispatch::Source::VNODE_WRITE
- Dispatch::Source::VNODE_EXTEND
- Dispatch::Source::VNODE_ATTRIB
- Dispatch::Source::VNODE_LINK
- Dispatch::Source::VNODE_RENAME
- Dispatch::Source::VNODE_REVOKE

Dispatch Source の種類として、以下のものを使用できます。
<table class="table">
<tr><th>定数名</th><th>GCD API での定義</th><th>内容</th></tr>
<tr>
  <td>Dispatch::Source::DATA_ADD</td>
  <td>DISPATCH_SOURCE_TYPE_DATA_ADD</td>
  <td><code>&lt;&lt;</code> メソッドを通じてイベントが発生し、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::DATA_OR</td>
  <td>DISPATCH_SOURCE_TYPE_DATA_OR</td>
  <td><code>&lt;&lt;</code> メソッドを通じてイベントが発生し、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::PROC</td>
  <td>DISPATCH_SOURCE_TYPE_PROC</td>
  <td>プロセスに関するイベントを受けると、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::SIGNAL</td>
  <td>DISPATCH_SOURCE_TYPE_SIGNAL</td>
  <td>シグナルを受けると、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::READ</td>
  <td>DISPATCH_SOURCE_TYPE_READ</td>
  <td>ファイルディスクリプタが読み込み可能になると、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::WRITE</td>
  <td>DISPATCH_SOURCE_TYPE_WRITE</td>
  <td>ファイルディスクリプタが書き込み可能になると、処理を実行します。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE</td>
  <td>DISPATCH_SOURCE_TYPE_VNODE</td>
  <td>ファイルが削除されたりファイルシステム上で変更があると、処理を実行します。</td>
</tr>
</table>

Dispatch::Source::PROC を使用する際に、以下の定数をマスクとして使用できます。
<table class="table">
<tr>
  <td>Dispatch::Source::PROC_EXIT</td>
  <td>DISPATCH_PROC_EXIT</td>
  <td>プロセスが終了した。</td>
</tr>
<tr>
  <td>Dispatch::Source::PROC_FORK</td>
  <td>DISPATCH_PROC_FORK</td>
  <td>子プロセスを作成した。</td>
</tr>
<tr>
  <td>Dispatch::Source::PROC_EXEC</td>
  <td>DISPATCH_PROC_EXEC</td>
  <td>exec や posix_spawn で別のプロセスを実行した。</td>
</tr>
<tr>
  <td>Dispatch::Source::PROC_SIGNAL</td>
  <td>DISPATCH_PROC_SIGNAL</td>
  <td>シグナルを送信した。</td>
</tr>
</table>

Dispatch::Source::VNODE を使用する際に、以下の定数をマスクとして使用できます。
<table class="table">
<tr>
  <td>Dispatch::Source::VNODE_DELETE</td>
  <td>DISPATCH_VNODE_DELETE</td>
  <td>ファイルなどが削除された。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_WRITE</td>
  <td>DISPATCH_VNODE_WRITE</td>
  <td>ファイルなどのデータが変更された。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_EXTEND</td>
  <td>DISPATCH_VNODE_EXTEND</td>
  <td>ファイルサイズが変更された。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_ATTRIB</td>
  <td>DISPATCH_VNODE_ATTRIB</td>
  <td>ファイルなどのメタ情報が変更された。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_LINK</td>
  <td>DISPATCH_VNODE_LINK</td>
  <td>The file-system object link count changed.</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_RENAME</td>
  <td>DISPATCH_VNODE_RENAME</td>
  <td>ファイル名が変更された。</td>
</tr>
<tr>
  <td>Dispatch::Source::VNODE_REVOKE</td>
  <td>DISPATCH_VNODE_REVOKE</td>
  <td>The file-system object was revoked.</td>
</tr>
</table>

Dispatch Source は他のクラスよりもわかりにくいと思います。MacRuby のリポジトリにある [source_spec.rb](https://github.com/MacRuby/MacRuby/blob/master/spec/macruby/core/gcd/source_spec.rb) を参考にしたりすると良いでしょう。

## Dispatch::Source クラスのメソッド
### Dispatch::Source.new

Dispatch Source を生成し返します。

- new(type, handle, mask, queue) { ... } -> Source
  - [PARAM] type:
	- Dispatch Source の種類を指定します。
  - [PARAM] handle:
	- イベントを監視するためのディスクリプタを指定します。type に <code>DATA_ADD</code> と <code>DATA_OR</code> を指定した場合には、<code>handle</code> は <code>0</code> とします。
  - [PARAM] mask:
	- マスクを指定できる Dispatch Source の種類を type で使用した際に指定します。
  - [PARAM] queue:
	- イベントが発生したときに、block の処理を追加する Dispatch Queue を指定します。
  - [RETURN]
    - 生成した Dispatch Source のインスタンスを返します。

### Dispatch::Source.timer
タイマーイベント用の Dispatch Source を生成し返します。

- timer(delay, interval, leeway, queue) { ... } -> Source
  - [PARAM] delay:
	- 何秒後からタイマーを開始するか指定します。nil の場合には Dispatch::TIME_NOW が指定されたのと同じになります
  - [PARAM] interval:
	- 何秒間隔で繰り返すか指定します。
  - [PARAM] leeway:
	- 何秒までの遅れを許容するか指定します。
  - [PARAM] queue:
	- イベントが発生したときに、block の処理を追加する Dispatch Queue を指定します。
  - [RETURN]
    - 生成した Dispatch Source のインスタンスを返します。

```ruby
gcdq = Dispatch::Queue.new('sample')
timer = Dispatch::Source.timer(0.5, 1, 0.5, gcdq) do |s|
  puts "Wake up!"
end

sleep 3
```

### Dispatch::Source#cancelled?
Dispatch Source がキャンセルされているか返します。

- cancelled? -> bool
  - [RETURN]
    - キャンセルされていれば true、そうでなれば false を返します。

### Dispatch::Source#cancel!
Dispatch Source をキャンセルします。

- cancel!

```ruby
gcdq = Dispatch::Queue.new('sample')
timer = Dispatch::Source.timer(1, 0.5, 0, gcdq) do |s|
  puts "Wake up!"
end

sleep 2

timer.cancel!
p timer.cancelled? # => true

sleep 1
```

### Dispatch::Source#handle
Dispatch Source の handle を返します。

- handle
  - [RETURN]
    - Dispatch Source の handle を返します。

### Dispatch::Source#mask
Dispatch Source の mask を返します。GCD API の `dispatch_source_get_mask` に相当します。

- mask
  - [RETURN]
    - Dispatch Source の mask を返します。

### Dispatch::Source#data
Dispatch Source でまだ処理されていないデータを返します。GCD API の `dispatch_source_get_data` に相当します。

- data
  - [RETURN]
    - Dispatch Source でまだ処理されていないデータを返します。

Dispatch Source の種類によって、以下のようなデータを返します。

<table class="table">
<tr>
  <td>Dispatch::Source::DATA_ADD</td>
  <td><code>&lt;&lt;</code>で送信されたデータ</td>
</tr>
<tr>
  <td>Dispatch::Source::DATA_OR</td>
  <td><code>&lt;&lt;</code>で送信されたデータ</td>
</tr>
<tr>
  <td>Dispatch::Source::PROC</td>
  <td>Dispatch::Source::PROC_EXIT、Dispatch::Source::PROC_FORK、Dispatch::Source::PROC_EXEC、Dispatch::Source::PROC_SIGNAL のいずれか</td>
</tr>
<tr>
  <td>Dispatch::Source::SIGNAL</td>
  <td>シグナルの番号</td>
</tr>
<tr>
  <td>Dispatch::Source::READ</td>
  <td>読み込み可能なバイト数</td>
</tr>
</table>


```ruby
gcdq = Dispatch::Queue.new('sample')
src = Dispatch::Source.new(Dispatch::Source::DATA_ADD, 0, 0, gcdq) do |s|
  puts s.data
end
src << 1
sleep 0.5
```

### Dispatch::Source#<<
Dispatch::Source::DATA_ADD と Dispatch::Source::DATA_OR を指定した Dispatch Source で使用します。指定したデータを Dispatch Source の block へ送信します。GCD API の `dispatch_source_get_data` に相当します。

- << data
