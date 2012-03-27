---
layout: post
title: "MacRuby 独自の定数"
date: 2012-03-26 21:00
comments: true
sharing: true
categories: MacRuby
---

ここでは MacRuby で追加されている定数について説明します。


## 独自の定数
### Kernel::RUBY_ARCH
動作している MacRuby が対象としている CPU のアーキテクチャ名。

```ruby
>> Kernel::RUBY_ARCH
=> "x86_64"
```


### Kernel::MACRUBY_VERSION
MacRuby のバージョン。

```ruby
>> Kernel::MACRUBY_VERSION
=> "0.12"
```

たとえば以下のようなメソッドを用意しておくと、実行している環境が MacRuby かそれ以外なのか簡単にチェックすることができます。
```ruby
def is_macruby?
  begin
    MACRUBY_VERSION
    return true
  rescue
    return false
  end
end
```


### Kernel::MACRUBY_REVISION
最後にコミットされたパッチの Git SHA1 ハッシュ値(古い MacRuby では SVN のリビジョン番号)。

```ruby
>> Kernel::MACRUBY_REVISION
=> "git commit e35df75944e7a13d16f019e2ed1e4ce2406b06af"
```


### Dir::NS_TMPDIR
テンポラリディレクトリのパス。

```ruby
>> Dir::NS_TMPDIR
=> "/var/folders/1z/ff7x15cj7vb24rl38ty0y52w0000gn/T/"
```
