# Cisco Meraki セキュリティ&SD-WAN(MXシリーズ)の設定例一覧

## 検証環境

本設定ガイドは、下記の機器およびファームウェアで動作確認を行っています。  
機種やソフトウェアのバージョンが異なる場合、画面表示や設定項目が一部変わることがあります。

| **項目** | **内容** |
| :--- | :--- |
| **ソフトウェアバージョン** | MX 18.211 |
| **検証機種** | MX67 |

## パスワード認証
* リモートアクセスVPN(L2TP/IPSec) グループによるアクセス制限 [標準](vpn-group-password.md) [^1]
* リモートアクセスVPN(Cisco Secure Client) グループによるアクセス制限 [標準](vpn-group-password-secureconnect.md) [^1]

## ２要素認証（パスワード＋クライアント証明書認証）
* リモートアクセスVPN(Cisco Secure Client) グループによるアクセス制限 [標準](vpn-group-password-cert-secureconnect.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 機器が送信しているRADIUSリクエストにNAS-ID属性はなく、NAS-IDには、機器のIPアドレスが含まれていないため、拡張RADIUSサーバは利用できません。
