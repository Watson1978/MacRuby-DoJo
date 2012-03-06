---
layout: post
title: "MacRuby の環境を構築しよう"
date: 2012-03-06 17:59
comments: true
sharing: true
categories: MacRuby
---

## 必要なもの
MacRuby を使用する際には、あらかじめ以下のものをインストールする必要があります。

- Mac OS X 10.6.8 以降
- Xcode 4.2 または Xcode 4.3
- MacRuby
- BridgeSupport Preview 3 (**Mac OS X 10.6.8 を利用する場合のみ必要**)


## Xcode をインストール
Xcode は Mac App Store からインストールできます。Mac App Store を起動し、右上の検索欄に Xcode と入力し検索します。

![image](/images/intro-install/search_xcode.png)

検索結果に表示された Xcode をインストールします。

![image](/images/intro-install/xcode.png)

使用している Mac OS X が 10.6.8 の場合には Xcode 4.2.x が、10.7.x の場合には Xcode 4.3.x がインストールされます。


## Command Line Tools をインストール
Xcode 4.3 からは、コマンドラインで実行するコンパイラなどのツール類を別途インストールしなければなりません。Xcode と併せてインストールしておきましょう。

Xcode のメニューから [Xcode]->[Preferences...] と選択していきます。下の図のように Downloads のタブを選択し、Command Line Tools をインストールします。

![image](/images/intro-install/command_line_tools.png)

## MacRuby をインストール
引き続き、[MacRuby](http://www.macruby.org/) をインストールしましょう。

![image](/images/intro-install/macruby_org.png)

現在リリースされている MacRuby 0.10 は、残念ながら Xcode 4.3 には対応していません。そこで [http://www.macruby.org/files/nightlies/](http://www.macruby.org/files/nightlies/) から nightly build の *macruby_nightly-latest.pkg* をダウンロードしてインストールします。nightly build は、毎日作られその時点での最新の変更内容が適用されたパッケージです。

![image](/images/intro-install/nightly_build.png)


## BridgeSupport Preview 3 をインストール
**Mac OS X 10.6.8 を利用している方のみ** BridgeSupport Preview 3 をインストールします。BridgeSupport は Mac OS X が提供する Framework の情報を取得するためのものです。Mac OS X 10.7 にはあらかじめ同等のものがインストールされています。[http://www.macruby.org/files/](http://www.macruby.org/files/) から *BridgeSupport Preview 3.zip* をダウンロードしインストールします。
