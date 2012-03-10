---
layout: post
title: "Objective-C のメソッドの読み方"
date: 2012-03-10 11:37
comments: true
sharing: true
categories: MacRuby
---

MacRuby でアプリケーションを作成しようとすると、Mac OS X の [API リファレンス](https://developer.apple.com/library/mac/navigation/) を読まなければなりません。API リファレンスを読む上で、Objective-C を MacRuby の規則に置き換えて読む必要があります。

## 文字列の表現
Objective-C では文字列のリテラル表現として `@""` を使います。Ruby では、単に `""` で表現できます。

- Objective-C
```objc
NSString *string = @"Hello World!";
```

- MacRuby
```ruby
string = "Hello World!"
```

## メソッドの呼び出し方
Objective-C ではメソッドを呼び出しを `[レシーバ メソッド]` という形式で記述します。また、引数の区切りに `,` は無く、単にスペースで区切ります。

- Objective-C
```objc
[NSString stringWithContentsOfURL: nsurl
                         encoding: NSUTF8StringEncoding
                            error: nil];
```

- MacRuby
```ruby
NSString.stringWithContentsOfURL(nsurl,
                                 encoding: NSUTF8StringEncoding,
                                 error: nil)
```

以下のような手順で Objective-C を MacRuby の文法に置き換えます。

1. `[レシーバ メソッド]` の `[]` を消す
2. 1 つ目の引数の前にある `:` を `(` に置き換える
3. 引数の区切りに `,` を使う
4. `;` を `)` で置き換える

`encoding:` や `error:` はキーワード引数を表していて、コロンの手前(`encoding`) までをキーワードと呼ばれます。(Ruby 2.0 で同様の文法がサポートされるようです。)


## メソッドの宣言

- Objective-C
```objc
- (id)foo:(NSString *)string index:(NSInteger)num {
}
```
- 一文字目の `-` はインスタンスメソッドであることを表しています。`+` の場合はクラスメソッドを表しています。
- `(id)` はメソッドの返り値の型情報を表しています。
- `(NSString *)` と `(NSInteger)` は引数の型情報を表しています。
  

- MacRuby
```ruby
def foo(string, index: num)
end
```

1. `-` とメソッドの返り値の型情報を `def` で置き換える
2. 引数の型情報を削除する
3. 1 つ目の引数の前にある `:` を `(` に置き換える
4. 引数の区切りに `,` を使う

という手順を踏むと、ほぼ MacRuby の文法になるかと思います。


----
TBD

他に気がついたら書き足します。
