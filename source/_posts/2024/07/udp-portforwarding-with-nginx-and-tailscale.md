---
title: TailscaleとNginxを活用してUDPポートフォワーディングをする
category:
  - 雑記
tags:
  - Tailscale
  - Nginx
  - Network
date: 2024-07-07 15:53:22
---

# 目次

- [目次](#%E7%9B%AE%E6%AC%A1)
- [免責事項](#%E5%85%8D%E8%B2%AC%E4%BA%8B%E9%A0%85)
- [なんでUDPポートフォワーディング?](#%E3%81%AA%E3%82%93%E3%81%A7udp%E3%83%9D%E3%83%BC%E3%83%88%E3%83%95%E3%82%A9%E3%83%AF%E3%83%BC%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0)
- [今回使用する環境](#%E4%BB%8A%E5%9B%9E%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B%E7%92%B0%E5%A2%83)
  - [サーバーPC](#%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BCpc)
  - [VPS](#vps)
- [準備](#%E6%BA%96%E5%82%99)
  - [Tailscaleのアカウントを登録する](#tailscale%E3%81%AE%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E7%99%BB%E9%8C%B2%E3%81%99%E3%82%8B)
  - [サーバーPC](#%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BCpc-1)
    - [1. Tailscaleのクライアントをインストール](#1-tailscale%E3%81%AE%E3%82%AF%E3%83%A9%E3%82%A4%E3%82%A2%E3%83%B3%E3%83%88%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
    - [2. ログイン](#2-%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3)
  - [VPS](#vps-1)
    - [1. Tailscaleをインストール](#1-tailscale%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
    - [2. tailscale upを実行する](#2-tailscale-up%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B)
    - [3. ブラウザでリンクを開いてデバイスを登録する](#3-%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E3%81%A7%E3%83%AA%E3%83%B3%E3%82%AF%E3%82%92%E9%96%8B%E3%81%84%E3%81%A6%E3%83%87%E3%83%90%E3%82%A4%E3%82%B9%E3%82%92%E7%99%BB%E9%8C%B2%E3%81%99%E3%82%8B)
    - [4. tailnet内のサーバーPCのIPを確認する](#4-tailnet%E5%86%85%E3%81%AE%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BCpc%E3%81%AEip%E3%82%92%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8B)
      - [Linux](#linux)
      - [Windows](#windows)
    - [5. NginxのStreamモジュールのコンフィグを書く](#5-nginx%E3%81%AEstream%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%81%AE%E3%82%B3%E3%83%B3%E3%83%95%E3%82%A3%E3%82%B0%E3%82%92%E6%9B%B8%E3%81%8F)
    - [6. Nginxを再起動](#6-nginx%E3%82%92%E5%86%8D%E8%B5%B7%E5%8B%95)
    - [7. エラーがなければおｋ](#7-%E3%82%A8%E3%83%A9%E3%83%BC%E3%81%8C%E3%81%AA%E3%81%91%E3%82%8C%E3%81%B0%E3%81%8A%EF%BD%8B)
    - [8. ポート開放をする](#8-%E3%83%9D%E3%83%BC%E3%83%88%E9%96%8B%E6%94%BE%E3%82%92%E3%81%99%E3%82%8B)
  - [最後に](#%E6%9C%80%E5%BE%8C%E3%81%AB)

# 免責事項

この方法は、素人が考えゴリ押しでやれることやった結果なのでセキュリティ、パフォーマンス、その他何かしらの問題が発生する可能性があります。

この方法を使用して発生したいかなるトラブルも筆者は一切の責任を負いかねます。

# なんでUDPポートフォワーディング?

Left 4 Dead 2やCounter Strike 2等のUDPを使用したゲームサーバーを身内で開いて遊びたいと常々考えているが、自宅のIPを公開したくないためVPS等でプロキシを構築しその通信を自宅サーバーに流したいと考えていた。
またポート開放が行えない環境の人も、VPSを活用することでゲームサーバーやUDPを使用したソフトウェアの外部公開ができるようになると思う。

# 今回使用する環境

## サーバーPC

ゲームするPCとサーバー用PCが同一なので、Windowsを使用していますが、実際はUbuntu等のLinuxでも問題ないです。

- Windows 10 22H2
- Tailscale
- UDPを使用する何か

## VPS

- Ubuntu 20.04
- Tailscale
- Nginx


# 準備



## Tailscaleのアカウントを登録する

[ログインページ](https://login.tailscale.com/login)にアクセスし、アカウントを作成します。

自分はGitHubのアカウントとの連携で登録しました。


## サーバーPC

### 1. Tailscaleのクライアントをインストール

[ダウンロードページ](https://tailscale.com/download)にアクセスし、対応したOSのインストール方法に従います。今回はWindowsを使用します。

### 2. ログイン

作ったアカウントでログインします。 

この操作はWindows等のGUIを使用する場合のみ使用する方法で、Linuxであれば`tailscale up`した際に出てくるリンクをブラウザで開いて認証します。

## VPS

### 1. Tailscaleをインストール

[ダウンロードページ](https://tailscale.com/download/linux)に書いてあるインストールスクリプトを使用してインストールします。

```sh
curl -fsSL https://tailscale.com/install.sh | sh
```

### 2. tailscale upを実行する

以下のコマンドを実行して、tailscaleをスタートします。

```sh
sudo tailscale up
```

### 3. ブラウザでリンクを開いてデバイスを登録する

コマンドを実行すると、以下のようなURLが出力されます。

```
To authenticate, visit:

        https://login.tailscale.com/a/xxxxxxxxxxx
```

リンクにアクセスして、登録したアカウントにログインし、`Connect`ボタンをクリックします。

### 4. tailnet内のサーバーPCのIPを確認する

#### Linux

Linuxであれば以下のコマンドtailnet内でのIPを確認できます。

```sh
tailscale up -4
```

#### Windows

システムトレイに格納されているTailscaleのアイコンを右クリックして This device: xxxxxx (xxx.xxx.xxx.xxx)と書かれてる()の部分を確認します。

### 5. NginxのStreamモジュールのコンフィグを書く

今回は簡易的な説明で済ましたいので、別のファイルには分けず `/etc/nginx/nginx.conf` をvim等お好きなエディタで編集します。

設定ファイルの末尾に、以下の内容を記述します。

```conf
stream {
    server {
        listen <port> udp;
        proxy_pass <ip>:<port>;
    }
}
```

- `listen <port> udp`の`<port>`部分に、VPS側で通信を待ち受けるポートを指定します。 例: Source Engine系のゲームであれば 27015
- `proxy_pass <ip>:<port>`の`<ip>`はtailnet内のサーバーPCのIPを、`<port>`にはtailnet内のサーバーPCが開いているサーバーのポートを指定します。

今回は、Left 4 Dead 2のサーバー用にセットアップしたいので以下のようにしました。

```conf
stream {
    server {
        listen 27015 udp;
        proxy_pass xxx.xxx.xxx.xxx:27015;
    }
}
```

### 6. Nginxを再起動

Ubuntuであれば以下のコマンドでできます。

```sh
sudo systemctl restart nginx
```

### 7. エラーがなければおｋ

エラーが発生していなければ、*:27015に対する接続がxxx.xxx.xxx.xxx:27015に転送されるようになります。

### 8. ポート開放をする

ufwであれば以下のコマンドでできます。

```sh
sudo ufw allow <port>/udp
sudo ufw reload
```

## 最後に

かなり便利な方法ではあるのですが、最初に言ったようにセキュリティ的にはおそらく問題ありそうなので、使わない時はtailscaleを落としておく等各自対策をお願いします。