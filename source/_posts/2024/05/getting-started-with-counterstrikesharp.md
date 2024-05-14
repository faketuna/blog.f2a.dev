---
title: CounterStrikeSharpの環境を構築してHello Worldしよう
category:
  - 雑記
tags:
  - C#
  - CounterStrike
  - CounterStrike2
  - CounterStrikeSharp
  - Modding
  - dotnet
date: 2024-05-13 15:20:06
---

# これを見る前に...

前提として[公式のドキュメント](https://docs.cssharp.dev/docs/guides/getting-started.html)見たほうが早いです。 英語読める人はこっち見るのおすすめ。

# とりあえず入れよう

筆者は本番環境がWindowsであるため開発環境もWindowsで構築しています。

## 自分の環境

### 開発

- Windows 10 22H2
- .NET 8.0.101
- Visual Studio Code


### テストサーバー

- VMWare Workstation
- Windows 11 23H2



# サーバーの環境構築

## 1. SteamCMDからCounterStrike2をインストールする

1. https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip から`steamcmd.zip`をダウンロード
2. `steamcmd.zip`を解凍
3. 中にある`steamcmd.zip`を実行
4. `force_install_dir <インストール先>`でインストール先を指定
5. `login anonymous` でログイン
6. `app_update 730` でインストール開始
7. 終わるまで待つ

## 2. Metamodを入れる

[ここ](https://www.sourcemm.net/downloads.php/?branch=master)から2.x系のMetamodをダウンロードしたら

1. Metamodを解凍して`game/csgo`ディレクトリに`addons`ディレクトリをコピペ
2. `game/csgo`ディレクトリの中にある`gameinfo.gi`を開く
3. `Game_LowViolence csgo_lv`の下に`Game csgo/addons/metamod`を追加する
4. サーバー起動
## 3. CounterStrikeSharpを入れる

1. [ここ](https://github.com/roflmuffin/CounterStrikeSharp/releases)から`counterstrikesharp-with-runtime-build-***-windows-*******.zip`をダウンロード
2. Metamodを解凍して`game/csgo`ディレクトリに`addons`ディレクトリをコピペ
3. サーバー起動


## 4. 確認

サーバーが起動したら`meta list`と打って、CounterStrikeSharpが出てくればOK



# 開発環境構築

## 1. .NET SDKを入れる

まず[ここ](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)から.NET 8.0のSDKをダウンロードします。

## 2. NETのクラスライブラリを作成する

以下のコマンドを使用して、クラスライブラリのプロジェクトを作成します。

```
dotnet new classlib --name <ProjectName>
```

## 3. 依存関係を追加する

`<ProjectName>.csproj`があるディレクトリで、以下のコマンドを使用してCounterStrikeSharpAPIを依存関係として追加します。

```
dotnet add package CounterStrikeSharp.API
```

## 4. とりあえずHello Worldしておく

以下のようなコードでHello World出来ます。

```csharp
namespace VeryNicePlugin {
    public class VeryNicePlugin: BasePlugin {
        public override string ModuleName => "Plugin Name";

        public override string ModuleVersion => "0.0.1";

        public override void Load(bool hotReload)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

## 5. コンパイル

1. `<ProjectName>.csproj`があるディレクトリで`dotnet build --configuration Release`を実行します。
2. 成果物が、`<ProjectName>\bin\Release\`以下に生成されます


## 6. サーバーに導入

`<ProjectName>.dll`をサーバーのPluginsにコピーしてあげれば勝手にホットリロードされる。

入れる際は、`plugins/<ProjectName>/<ProjectName>.dll`としないと読み込まれないので注意