---
title: VRChatのワールドをテストしたい時にwin32exceptionが出てBuild & Test new Buildがうまく行かない
category:
  - Unity
tags:
  - VRChat
  - world
  - error
  - troubleshoot
  - unity
date: 2024-01-10 13:29:56
---

# 結論

`Build & Test new Build`じゃなくて大人しく`Build and Upload`すれば解決した

多分壊れてるのか知らないけど、どう頑張っても`Build & Test new Build`が自分の環境だと動かなかったので大人しくアップロードしてテストしよう

# 追記 2024-02-19

VRChatがCドライブ以外にインストールされてるとこのエラーが発生するみたい。Cドライブにインストールし直したらエラーは出なくなったが、自分の環境だとテスト用のVRChatは起動しなかった。