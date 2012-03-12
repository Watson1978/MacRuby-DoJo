---
layout: post
title: "Window のリサイズに対応しよう"
date: 2012-03-08 22:52
comments: true
sharing: true
categories: MacRuby
---

作成したアプリケーションを実行した後、Window がリサイズされることを考慮しましょう。何も対処していなければ、以下のようにリサイズすると間が抜けた UI レイアウトになったり、たくさんの UI 部品を使用したアプリケーションでは使用するのが困難なほどにレイアウトが崩れたりすることもあるでしょう。

![image](/images/ja/intro-window-resize/window.png)

## Autosizing の設定
Window をリサイズした時に、UI 部品の配置をどのように取り扱うかについて Xcode の Autosizing という項目で簡単に設定することができます。以下の図のように、Autosizing を設定する UI 部品を選択し Size Inspector を表示します。

![image](/images/ja/intro-window-resize/size_inspector.png)

幅や高さを伸縮させたい場合には以下のように設定します。

![image](/images/ja/intro-window-resize/autosizing_variable.png)

UI 配置位置を固定する場合には以下のように設定します。設定した箇所の間隔は、Window をリサイズしても変わらないようになります。

![image](/images/ja/intro-window-resize/autosizing_fixable.png)

Autosizing の効果は Example にプレビューされるので、確認しながら設定していくと良いでしょう。


## Window をリサイズできないようにする
単純な UI の場合ですと、そもそも Window のリサイズしなくても良いということもあるでしょう。

Window を選択した状態で、Size Inspector を表示します。

![image](/images/ja/intro-window-resize/window_size.png)

[Minimum Size] と [Maximum Size] のチェックを ON にすると、Window の最大最小サイズを固定できます。上の図のように Minimum Size と Maximum Size を同じ値にすると、Window がリサイズできないようになります。