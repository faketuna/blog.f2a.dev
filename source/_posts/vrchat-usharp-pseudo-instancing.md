---
title: UdonSharpで擬似的に新しいインスタンスを作りたい
category:
  - Unity
tags:
  - VRChat
  - Unity
  - C#
  - U#
  - UdonSharp
date: 2024-02-13 21:37:08
---

# UdonSharpで擬似的に新しいインスタンスを作りたい

## 何のために?

UdonSharpはnew Class()で新しいインスタンスを作れない(コンストラクタが作れない)から、通常の方法ではプレイヤー情報クラスなど、情報を保持させるためだけのクラスをnewして使うことが出来ない。

そうなると、一つのクラスにすべての情報を保持する必要が出てくるため管理が面倒になる。

# TL;DR

対象のスクリプトをコンポーネントとしてアタッチしたObjectを`Object.Instantiate()`を使用してクローンすれば擬似的にnew Class()したのと同じ効果を得られる。

# やり方

## 情報保持するクラスを作る

今回は簡易的に以下のようなテキストを保持するクラス`InstantiateTest`を実装する。

```csharp

using UdonSharp;
using UnityEngine;
using VRC.SDKBase;
using VRC.Udon;

public class InstantiateTest : UdonSharpBehaviour
{

    private string text = "<ERROR>";

    public void setText(string txt) {
        text = txt;
    }

    public string getText() {
        return text;
    }
}
```

### スクリプトをオブジェクトに割り当てる

今回は`ヒエラルキー右クリック -> Create Empty`でEmptyのオブジェクトを作成し、そこにUdonBehaviourコンポーネントをアタッチして先程作った`InstantiateTest`をUdon scriptとしてアタッチしてあげる。

オブジェクトの名前は今回は`InstantiateTestTarget`とする。

なお実際に使用する際は`PlayerInformationBase`等のようにわかりやすい名前にするのを推奨する。

## オブジェクトをクローンして疑似newする

今回は10個の`InstantiateTest`クラスを擬似的にnewして、値を設定、そして表示してみます。

### ベースとなるオブジェクトを探す

まずは、先程作成した`InstantiateTestTarget`オブジェクトを探す必要があるため

`GameObject.Find(string Name)`でオブジェクトを探す。

```csharp
GameObject obj = GameObject.Find("InstantiateTestTarget");
```

### オブジェクトをクローンする

`Object.Instantiate()`を使用してクローンできます。この関数は複数オーバーロードが存在します。詳細は[ここからどうぞ](https://docs.unity3d.com/ja/2020.3/ScriptReference/Object.Instantiate.html)

今回は10個作るので以下のようなコードで配列を作りオブジェクトを保持します。

```csharp
for(int i = 0; i < objs.Length; i++) {
    objs[i] = Instantiate(obj);
}
```

### InstantiateTestの変数を設定してみる

for文を回して、アタッチされている`InstantiateTest`を取得、その後`setText()`をします。

```csharp
for(int i = 0; i < objs.Length; i++) {
    if(objs[i].GetComponent<InstantiateTest>() != null) {
        objs[i].GetComponent<InstantiateTest>().setText($"Object number {i}");
    }
}
```

### InstantiateTest内の設定された値を確認してみる

今回もまたfor文を回して、`objs[]`の中にアタッチされている`InstantiateTest`を取得、`getText()`を呼び出して値を入手します。

その情報を`Debug.Log()`でログとして流してあげます。

```csharp
for(int i = 0; i < objs.Length; i++) {
    if(objs[i].GetComponent<InstantiateTest>() != null) {
        Debug.Log($"Reading text on object {objs[i].GetComponent<InstantiateTest>().getText()}");
    }
}
```

### ログを確認する

`Debug.Log()`の内容は以下の2つの方法で確認することが出来ます。

1. VRChatの起動引数に`--enable-debug-gui`を入れてゲーム内で``` ` + R SHIFT + 3 ```を押してデバッグコンソールを開く
2. VRChatの起動引数に`--enable-udon-debug-logging`を入れてUnityのコンソールから確認する

# 出来たコード

### InstantiateTestクラス

```csharp

using UdonSharp;
using UnityEngine;
using VRC.SDKBase;
using VRC.Udon;

public class InstantiateTest : UdonSharpBehaviour
{

    private string text = "<ERROR>";

    public void setText(string txt) {
        text = txt;
    }

    public string getText() {
        return text;
    }
}
```

### InstantiateTestCloneObjectクラス

```csharp

using UdonSharp;
using UnityEngine;
using VRC.SDKBase;
using VRC.Udon;

public class InstantiateTestCloneObject : UdonSharpBehaviour
{
    private bool alreadyInstantiated = false;
    public override void Interact()
    {
        if(alreadyInstantiated)
            return;
        
        alreadyInstantiated = true;

        Debug.Log("Finding object to instantiate");
        GameObject obj = GameObject.Find("InstantiateTestTarget");
        GameObject[] objs = new GameObject[10];
        
        Debug.Log("Instantiating objects");
        for(int i = 0; i < objs.Length; i++) {
            Debug.Log($"Instantiating object {i}");
            objs[i] = Instantiate(obj);
        }

        Debug.Log("Setting text on objects");
        for(int i = 0; i < objs.Length; i++) {
            if(objs[i].GetComponent<InstantiateTest>() != null) {
                Debug.Log($"Setting text on object {i}");
                objs[i].GetComponent<InstantiateTest>().setText($"Object number {i}");
            }
        }

        Debug.Log("Reading text");
        for(int i = 0; i < objs.Length; i++) {
            if(objs[i].GetComponent<InstantiateTest>() != null) {
                Debug.Log($"Reading text on object {objs[i].GetComponent<InstantiateTest>().getText()}");
            }
        }
    }
}
```