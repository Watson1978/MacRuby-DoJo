---
layout: post
title: "アイコンを変更しよう"
date: 2012-03-09 16:10
comments: true
sharing: true
categories: MacRuby
---

作成したアプリケーションのアイコンには Mac OS X であらかじめ用意されているものが使用されます。すべてのアプリケーションは、最初に同じアイコンで表示されることになり、あまり見栄えがしません。

![image](/images/ja/intro-icon/default_icon.png)

アプリケーションのアイコンを独自のものに変更してみましょう。

## Icon Composer
あらかじめ PNG 形式などで、アプリケーションに設定するアイコン用の画像を用意します。用意した画像を、アイコンは Icon Composer を使用しアプリケーションに設定できる形式に出力します。この Icon Composer は Xcode と一緒にインストールされています。

Xcode 4.3 からは、[Xcode]->[Open Developer Tool] から起動することができます。

![image](/images/ja/intro-icon/icon_composer_xcode43.png)

Xcode 4.2 以前では、*/Developer/Applications/Utilities/Icon Composer.app* というフォルダ内に存在します。

![image](/images/ja/intro-icon/icon_composer_xcode42.png)

Icon Composer を起動すると、アイコンサイズごとに枠がいくつか表示されています。その枠に用意しておいた画像をドラッグ & ドロップして配置し、アイコンを保存します。

![image](/images/ja/intro-icon/icns.png)

## アイコンを変更する
Xcode でアプリケーションのアイコンを変更します。以下の図のように、ターゲットを選択して [App Icon] に先ほど Icon Composer で作成したアイコンをドラッグ & ドロップします。

![image](/images/ja/intro-icon/change_icon.png)