---
title: VRChatで既存アバターに追加したOSCギミックが動かない！
category:
  - Unity
tags:
  - VRChat
  - Unity
  - OSC
  - troubleshoot
date: 2024-02-26 03:05:46
---

# 原因

すでにアップロードして使用したアバターは、OSCのパラメータ情報が書かれたファイルが生成済みでアップデートされることがないため、追加で入れたOSCの情報が上手く伝達されないのが原因です。

# 直し方

{% raw %}
<h2 style="color: #e35c5c">注意: 以下の内容は問題の解決を保証するものではなく、行う際は完全に自己責任の上でお願い致します。何か問題が発生しても、責任は一切負いません。</h2>
{% endraw %}

## 1. WIN + Rを押す

`ファイル名を指定して実行`というアプリケーションを開きます。

{% raw %}
<img style=" display: inline-block" src="/images/posts/2024/02/winr.png">
{% endraw %}


## 2. 以下の文字列をコピペしてOKを押す

`%localappdata%low\VRChat\VRChat\OSC` を入力欄に入れてOKを押します。

{% raw %}
<img style=" display: inline-block" src="/images/posts/2024/02/winrcnp.png">
{% endraw %}

## 3. フォルダを削除する

`usr_******`フォルダをゴミ箱に入れるなどして削除します。

{% raw %}
<img style=" display: inline-block" src="/images/posts/2024/02/delfolder.png">
{% endraw %}

## 4. アバターを切り替える

一度別のアバターに切り替えて、再度OSCギミックを追加したアバターに切り替えます。

問題がなければOSCのギミックが動くようになっているはずです。