---
layout: post
title: "Original Constants in MacRuby"
date: 2012-03-31 16:00
comments: true
sharing: true
categories: MacRuby
---

This content describes the constants that have been added in MacRuby.


## Original Constants
### Kernel::RUBY_ARCH
CPU name of the architecture which MacRuby is ran.

```ruby
>> Kernel::RUBY_ARCH
=> "x86_64"
```


### Kernel::MACRUBY_VERSION
MacRuby version.

```ruby
>> Kernel::MACRUBY_VERSION
=> "0.12"
```

Following method may check whether running your program on MacRuby.
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
Git SHA1 Hash which indicates a last commit. (Older MacRuby shows a revision number of SVN)

```ruby
>> Kernel::MACRUBY_REVISION
=> "git commit e35df75944e7a13d16f019e2ed1e4ce2406b06af"
```

### Dir::NS_TMPDIR
Path of temporary directory.

```ruby
>> Dir::NS_TMPDIR
=> "/var/folders/1z/ff7x15cj7vb24rl38ty0y52w0000gn/T/"
```