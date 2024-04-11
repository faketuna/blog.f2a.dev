---
title: Arturia社製 Minilab mk3のMIDI関連でちょっと詰まった
category:
  - DTM
tags:
  - Arturia
  - FL Studio
  - DAW
  - MIDI
date: 2024-01-04 08:04:50
---

# 何があったか

購入後にFL StudioでMiniLab mk3を使うとしたら。ExpressionとPitch bendが動かなかった。

Analog Lab Vのエフェクト等の操作もフェーダーやノブで出来るはずだったのに出来なかった。

## 環境

* Windows 10 22H2
* FL Studio 21
* Arturia MiniLab mk3


# TL;DR

ExpressionとPitch bendが動かない問題は[Arturiaの公式サイト](https://www.arturia.com/products/hybrid-synths/minilab-3/resources)にあるDaw Integration Scriptを導入

Analog Lab Vのエフェクト操作などをフェーダーやノブで出来るようにするには、MIDI Input portを10に設定し、MIDIキーボード側のモードをArturiaにすればOK



# Daw Integration Scriptを導入する

今回はFL Studioで進めていくので他のDAWの方は各自調べながら読み替えてください。

## 1. Daw Integration Scriptをダウンロードする

[ここ](https://www.arturia.com/products/hybrid-synths/minilab-3/resources)の下の方にあるDaw Integration Scriptから自分のDAWに対応するものをダウンロードする。

{% asset_img "arturia-dl.png" "arturia-dl-script-pic" %}

## 2. ScriptをFL StudioのScriptディレクトリに入れる

ダウンロードしたZIPファイルを解凍し、解凍したZIPファイル内にある`FL Studio Scripts`ディレクトリ下にある`MiniLab3`ディレクトリを以下のディレクトリに入れてあげる

Windowsの場合は
`C:\Users\<ユーザー名>\Documents\Image-Line\FL Studio\Settings\Hardware\`

Macの場合は
`/Users/<ユーザー名>/Documents/Image-Line/FL Studio/Settings/Hardware/`

## 3. MIDIのController Typeを変更する

MIDI Settingsを開いて、Inputの欄から`MiniLab3 MIDI`を選択し`Controller type`を`MiniLab3`か`Arturia MiniLab 3`を選択

{% asset_img "arturia-midi-setting.png" "arturia-midi-setting" %}

## 4. Expressionとかが動いてるか確認

MIDIキーボード側のモードを`SHIFT+PAD 3`でDAWに変更し、動いていれば完了！


# Analog Lab Vのエフェクト等の操作を出来るようにする

FL Studioで音源の設定を開いて、MIDIのInput portを10に設定してあげればOK。

あとは、MIDIキーボード側のモードを`SHIFT+PAD 3`でArturiaに変更して動いていれば完了！

{% asset_img "arturia-alab-midiset-1.png" "arturia-alab-midiset" %}