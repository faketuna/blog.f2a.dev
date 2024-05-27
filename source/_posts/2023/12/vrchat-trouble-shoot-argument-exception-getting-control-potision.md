---
title: "アバタービルド時にArgumentException: Getting control ?’s position in a group with only ? controls when doing repaintが出た時の対処方法"
category:
  - Unity
tags:
  - error
  - troubleshoot
  - unity
  - VRChat
date: 2023-12-19 01:33:37
---

{% raw %}
<h1 style="color: #e35c5c">注意!!</h1>
{% endraw %}

{% raw %}
<h2 style="color: #e35c5c">以下の内容を行う前に必ずプロジェクトのバックアップを取ってください。また、以下の手順を行って発生したいかなる損害も責任は負いません。</h2>
<h2 style="color: #e35c5c">また以下の内容はトラブルの改善を約束するものではありません。</h2>
<h2 style="color: #59b3e3">キセテネを使用して衣装導入した方は対処法4から読んでください</h2>
{% endraw %}


新しく衣装を導入した際などにこのエラーが出ることがあるらしい。たまたま周りの方が遭遇していたため気になって調べてみたのでメモ

---

## 前提

問題が発生したらとりあえず再起動しようね。


---

## 対処法1

とりあえずバックアップからやり直してみようのコーナー

1. バックアップを復元する
2. 再度同じことをする

それでもだめだった時は2へどうぞ

---

## 対処法2

1. 改変をする前とエラーが発生するようになった間に入れたゲームオブジェクトのInspectorを確認
2. Skinned Mesh RendererのMaterialsプロパティを確認
3. プロパティに0が入っていた場合はゲームオブジェクトを削除する

もしそのオブジェクトが動作に必要もしくは服の本体などである場合は3へどうぞ

{% raw %}
<img style=" display: inline-block" src="/images/posts/2023/12/vrchat-ex-method-2.png">
{% endraw %}

---

## 対処法3

1. 改変をする前とエラーが発生するようになった間に入れたゲームオブジェクトのInspectorを確認
2. Skinned Mesh RendererのMaterialsプロパティを確認
3. Materialsがしっかり設定されていることを確認
4. Meshがmissingになっていることを確認
5. 対応するメッシュを設定してあげるか、再度インポートして正常な状態の復元を試みる

{% raw %}
<img style=" display: inline-block" src="/images/posts/2023/12/vrchat-ex-method-3.png">
{% endraw %}

---

## 対処法4

もしもキセテネ使ってるなら...

キセテネを使用して洋服を着せた後に、洋服側の中に残る子オブジェクトが存在しないarmatureが残っていることがあります。
この残ったarmatureを削除すると治る可能性があります。

{% raw %}
<img style=" display: inline-block" src="/images/posts/2023/12/vrchat-ex-method-kisetene.png">
{% endraw %}

# 参考元

https://prfac.com/vrm-export-error/