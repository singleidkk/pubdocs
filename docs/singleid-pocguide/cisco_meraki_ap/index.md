# Cisco Meraki 無線LANアクセスポイントの設定例一覧

## 検証環境

本設定ガイドは、下記の機器およびファームウェアで動作確認を行っています。  
機種やソフトウェアのバージョンが異なる場合、画面表示や設定項目が一部変わることがあります。

| **項目** | **内容** |
| :--- | :--- |
| **ソフトウェアバージョン** | MR 30.5 |
| **検証機種** | MR28 |

## クラウドRADIUSと連携可能な製品の確認方法
[メーカーの製品ページ](https://meraki.cisco.com/ja-jp/products/wi-fi/models/){ target=_blank }から、各モデルの**Product Datasheet**へアクセスし、**Security**の項目に**EAP-TLS**が記載されている製品です。

## パスワード認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-password.md)/[拡張](wlan-group-password-adv.md) [^1]

## クライアント証明書認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-cert.md)/[拡張](wlan-group-cert-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。
