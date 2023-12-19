---
title: mi band 7(Xiaomi smart band 7)とiPhoneでpulsoidに心拍数を送る方法
category:
  - 雑記
tags:
  - pulsoid
  - miband
  - iphone
  - heartrate
date: 2023-12-18 13:31:03
---

# 前書き
OBSで配信に, VRChatでアバターに心拍数を表示したら面白そうだな～と思ったので、手元のmi band 7を使ってPulsoidに心拍数を送ろうとしてつまづいたのでメモ

(Androidだともっと楽な方法があるかも)

## 必要なもの

* スマホ (自分の環境は iPhone Xs (ios 16.7))
* Xiaomi Smart band 7
* Zepp life
* Pulsoid

## 手順

### 前提
pulsoidのアカウント作成等は他のデバイスでやるのと同じ方法なのとネットに情報がたくさんあるので省略します。
スマホにpulsoidとzepp lifeを導入してログインとペアリングを済ました前提で話を進めます。

### Zepp lifeで必要な設定を行う

#### 1. Zepp lifeを開きプロフィールをタップ

#### 2. マイデバイスから自分のデバイスを選択

#### 3. 下にスクロールして、以下の２つを有効化
* 検出可能
* アクティビティ心拍数の共有

### Pulsoidに心拍数を送る

#### 1. Zepp lifeを開きワークアウトを選択

#### 2. GOを押してワークアウトを開始 (ウォーキングでもランニングでもok)

#### 3. Smart band側でワークアウトの開始通知が出たのを確認

#### 4. Pulsoidを開く

#### 5. 最初の画面にはおそらく何も出ないので、`Scan for all BLE devices`を選択

#### 6. 自分のデバイスを選択。 (もしかしたら末尾のIDだけが違うデバイスが2個あるかもしれませんが、どっちかが繋がります)

#### 7. おわり

## 情報元
https://www.reddit.com/r/miband/comments/vs28ky/mi_band_7_heart_rate_data_sharing_isnt_working/

---

# 2023-12-20 追記

## ワークアウト中でなくても計測する

[Lappy_Error404](https://twitter.com/Lappy_Error404)さん情報ありがとうございます!

1. スマートウォッチの方でなんでも良いのでワークアウトを開始しておく
2. Zepp appの方でワークアウトを開始する
3. Pulsoidに繋げる
4. zepp app -> スマートウォッチの順番でワークアウトを切る

これで次回以降はPulsoidからいつものようにスマートウォッチに繋げるだけで心拍数を取ってくるようになる

ただしBluetoothとかZeppapp等でスマートウォッチの検出ができなくなると再度やり直す必要あり

もし途中で接続できなくなった場合はスマートウォッチ側でワークアウトを開始した後にPulsoid側で再接続すると直る事がある