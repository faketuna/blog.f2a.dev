---
title: ScriptからUIのTextMeshProコンポーネントをいじる時につまずいた話
category:
  - Unity
tags:
  - Unity
  - VRChat
  - C#
  - U#
  - UdonSharp
date: 2024-02-09 21:10:31
---

# TL;DR

ヒエラルキー右クリック -> UI -> TextMeshProの順で追加したTextMeshProは、型が`TextMeshPro`ではなく`TextMeshProUGUI`になる。


# 何があったか

Unityのヒエラルキーを右クリックして UI -> TextMeshProとコンポーネントを追加した。

その後、以下の様なコードでScriptからTextMeshProのテキストを変更しようと頑張っていたのだがどう頑張ってもコンポーネントが取得できなかった。

```csharp
foreach(var obj in gameObject) {
    if(obj.GetComponent<TextMeshPro>() != null) {
        obj.GetComponent<TextMeshPro>().text = "foo";
    }
}
```

# どうやって解決した?

そもそも型が違った。 UI -> TextMeshProで作成されたコンポーネントは`TextMeshPro`じゃなくて`TextMeshProUGUI`というやつだったらしい。

よって修正後のコードはこうなる

```csharp
foreach(var obj in gameObject) {
    if(obj.GetComponent<TextMeshProUGUI>() != null) {
        obj.GetComponent<TextMeshProUGUI>().text = "foo";
    }
}
```

Unityとの戦争は続きそうだ