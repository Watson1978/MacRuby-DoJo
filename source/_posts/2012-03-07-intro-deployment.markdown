---
layout: post
title: "Deployment してアプリケーションを配布しよう"
date: 2012-03-07 17:50
comments: true
sharing: true
categories: MacRuby
---

作成したアプリケーションは MacRuby がインストールされている環境でしか動作しません。アプリケーションを配布する際には、MacRuby がインストールされていない環境で利用されるかもしれません。

そこで MacRuby には、アプリケーションに MacRuby 自体を埋め込む仕組みが備わっています。これにより MacRuby がインストールされていない環境でもアプリケーションが動作します。

以下の図のように「Deployment」を選択した状態で [Run] をクリックすると、アプリケーションに MacRuby が埋め込まれます。

![image](/images/intro-deployment/deployment.png)

Deployment ではいくつかのオプションを指定することができます。以下の図のように「Arguments」欄でオプションを指定します。

![image](/images/intro-deployment/deployment_option.png)

指定できるオプションは、`macruby_deploy` コマンドを Terminal で実行することで確認できます。

```
$ macruby_deploy 
Usage: macruby_deploy [options] application-bundle
        --compile                    Compile the bundle source code
        --embed                      Embed MacRuby inside the bundle
        --no-stdlib                  Do not embed the standard library
        --stdlib [LIB]               Embed only LIB from the standard library
        --gem [GEM]                  Embed GEM and its dependencies
        --bs                         Embed the system BridgeSupport files
        --verbose                    Log all commands to standard out
        --codesign [CERT]            Sign the files with the specified certificate
    -v, --version                    Display the version
```

<table class="table">
<tr><th>オプション</th><th>内容</th></tr>
<tr><td>--compile</td><td>アプリケーション内で使用している `*.rb` ファイルをコンパイルします。配布先で、不正にアプリケーションコードをコピーされるのを防ぐことができます</td></tr>
<tr><td>--embed</td><td>アプリケーションに MacRuby を埋め込みます</td></tr>
<tr><td>--no-stdlib</td><td>MacRuby の標準ライブラリを埋め込みません。標準ライブラリを使用していないアプリケーションでは、このオプションによってアプリケションのサイズを削減することができます</td></tr>
<tr><td>--stdlib ライブラリ名</td><td>指定したライブラリのみ埋め込みます。--stdlib json のように使用します。</td></tr>
<tr><td>--gem RubyGems名</td><td>指定した RubyGems をアプリケーションに埋め込みます</td></tr>
</table>

できあがったアプリケーションは、[Products] のアプリケーション名を <kbd>control</kbd> キーを押しながらクリックすると Finder で確認することができます。

![image](/images/intro-deployment/show_finder.png)

あとは、zip ファイルに圧縮するなどして配布してみてください。