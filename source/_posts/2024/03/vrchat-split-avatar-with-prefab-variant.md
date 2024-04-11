---
title: アバターをPrefab Variant化してギミックはそのままに衣装ごとにアバターを分ける
category:
  - Unity
tags:
  - VRChat
  - Unity
  - Prefab
  - Prefab variant
date: 2024-03-24 16:47:56
---

ギミックは共有しつつも容量削減等の理由で洋服単位でアバターを分けたいって思ったことありますよね? 私はあります。

その悩みを解決する方法を今日は書いていきたいと思います。

## 目次

- [目次](#目次)
- [Prefab Variantとは?](#Prefab-Variantとは)
- [なにはともあれやってみよう](#なにはともあれやってみよう)
  - [1. 改変用のPrefabを作る](#1-改変用のPrefabを作る)
  - [2. 作ったPrefabを改変する](#2-作ったPrefabを改変する)
  - [3. 改変したPrefabを上書き保存する](#3-改変したPrefabを上書き保存する)
  - [4. 洋服なしのPrefab Variantを作る (任意)](#4-洋服なしのPrefab-Variantを作る-任意)
  - [5. ベースとなるPrefabからPrefab Variantを作成する](#5-ベースとなるPrefabからPrefab-Variantを作成する)
  - [6. 作ったPrefab Variantに洋服を着せる](#6-作ったPrefab-Variantに洋服を着せる)
  - [7. あれ? ギミックは...?](#7-あれ-ギミックは)
  - [8. ベースのPrefabにギミックを追加する。](#8-ベースのPrefabにギミックを追加する)
  - [9. Prefab Variantの方にも変更が適用されているか確認する](#9-Prefab-Variantの方にも変更が適用されているか確認する)
  - [10. アップロード](#10-アップロード)
  - [おわり](#おわり)

## Prefab Variantとは?

Unity公式では以下のように説明されています。

> プレハブバリアントは、一揃いの事前定義されたプレハブのバリエーションを使用したい場合に便利です。

どういう事？ってなるかもしれませんが要は、中身はベースのPrefabとほぼ一緒だけど一部だけ変えたい時とかに有用ってことです。

今回であればアバターのギミックや髪の色, 素体等は共通のまま、洋服だけ変えたい... けど全部に個別にやるとなると面倒くさい！そんな時に役立ちます。

## なにはともあれやってみよう

アバターは**わこーのあとりえ**さんの[レイ](https://booth.pm/ja/items/4360868)ちゃんを使用させていただきました。 みんなもレイちゃんになろう！！

### 1. 改変用のPrefabを作る

まずは最初にベースとなる改変用のPrefabを作りましょう。 

作ると言ってもやることはものすごく簡単です。

1. 元のアバターをヒエラルキーに入れる
2. ヒエラルキーにあるアバターをPrefabを保存したい場所でドラッグアンドドロップ
3. Original Prefabとして保存

<img style=" display: inline-block" src="/images/posts/2024/03/create-prefab.gif">


### 2. 作ったPrefabを改変する

髪の色やアクセサリーなど、ベースとなる部分を改変していきます。

今回自分は、髪飾りとパーティクルを追加し、髪と服の色を変更しました。

<img style=" display: inline-block" src="/images/posts/2024/03/ray-after-edit.png">


### 3. 改変したPrefabを上書き保存する

{% raw %}
<p style="color: #e35c5c">注意: 強制ではありませんが、アバターのオブジェクトのチェックは入れたままOverrideするのをおすすめします。</p>
{% endraw %}

1. アバターのオブジェクトを選択
2. インスペクタ上部に表示されている`Overrides`をクリック
3. `Apply All`をクリック

<img style=" display: inline-block" src="/images/posts/2024/03/override-base-prefab-1.gif">

### 4. 洋服なしのPrefab Variantを作る (任意)

これは個人の自由ですが、自分はDLサイズやポリゴン数, テクスチャメモリの使用量を減らすために別の洋服を入れる場合はデフォルトの洋服を消しています。

入れっぱなしでもいいかな～って人は`5`まで飛ばしていただいて構いません。

1. 保存したPrefabファイルを右クリック
2. Create -> Prefab Variantを選択
3. わかりやすい名前をつけておく
4. 洋服のメッシュを消す
5. Prefabを上書き保存する

<img style=" display: inline-block" src="/images/posts/2024/03/create-variant-with-no-clothe.gif">

### 5. ベースとなるPrefabからPrefab Variantを作成する

4を行った方は4で作ったPrefab Variantを、4を飛ばした方は1で作ったPrefabを選択してください。

1. ベースのPrefabファイルを右クリック
2. Create -> Prefab Variantを選択
3. わかりやすい名前をつけておく。 今回は`Ray Variant Jersey`とします。

<img style=" display: inline-block" src="/images/posts/2024/03/create-variant-for-new-clothe.gif">

### 6. 作ったPrefab Variantに洋服を着せる

作ったPrefab Variantをヒエラルキーに入れ、普段通り洋服を入れます。 (洋服は**かぷちや**さんの[ねこねこじゃーじ](https://capettiya.booth.pm/items/4782886)を使用させていただきました)

Modular Avatar対応衣装であればポン入れ、その他のツールであればツールの使い方通りに導入すれば完了です。

<img style=" display: inline-block" src="/images/posts/2024/03/ray-after-edit-2.png">

### 7. あれ? ギミックは...?

さて... 一通りの改変が終わり、まだギミックを入れてない事に気づいたと思います。

ここからが、Prefab Variantが真価を発揮するときです。

### 8. ベースのPrefabにギミックを追加する。

今回は、**まるるの魔法雑貨屋さん**の[AvatarBasicTool - ABT](https://booth.pm/ja/items/5001784)を導入します。

基本的にどんなギミックでも、改変対象のアバターに対応しているものであれば大丈夫です。(多分 きっと Maybe Probably)

1. ベースのPrefabのオブジェクトに任意のギミック(今回はABT)をドラッグアンドドロップ
2. アバターのオブジェクトを選択
3. インスペクタ上部に表示されている`Overrides`をクリック
4. `Apply All`をクリック

<img style=" display: inline-block" src="/images/posts/2024/03/add-gimmick-to-base-prefab.gif">

### 9. Prefab Variantの方にも変更が適用されているか確認する

1. Prefab Variant(今回の場合は`Ray Variant Jersey`)のオブジェクトをクリック
2. 任意のギミック(今回はABT)が導入されていればOK!

<img style=" display: inline-block" src="/images/posts/2024/03/add-gimmick-confirm.gif">

### 10. アップロード

後は、普段通りにアバターのサムネイルや名前を設定してアップロードするだけです！

今後ベースのPrefabからギミックを追加/更新/削除した場合は、Prefab Variantのアバターをアップロードし直すだけで変更が適用されます。

### おわり

Prefab Variantを使うと、ベースのPrefabをいじるだけでそこから派生したPrefab Variant全てに変更を適用できちゃうのでまじで時短できてたすかる。

みんなもPrefab Variantを使って、洋服が大量に入ってて超重い女から重い女になりましょう。