# Aruba Instant 無線LANアクセスポイントの設定例一覧
文書更新日:2026-08-05

## 検証環境

本設定ガイドは、下記の機器およびファームウェアで動作確認を行っています。
機種やソフトウェアのバージョンが異なる場合、画面表示や設定項目が一部変わることがあります。

| **検証機種** | **ソフトウェアバージョン** |
| :--- | :--- |
| AP-505-JP（R2H27A） | Aruba Instant 8.7.1.11（build 84926） |
| AP-505-JP（R2H27A） | Aruba Instant 8.13.3.0 LSR（build 96306） |
| AP-615-JP（R7J48A） | Aruba Instant 8.12.0.0 SSR（build 89362） |
| AP-615-JP（R7J48A） | Aruba Instant 8.13.3.0 LSR（build 96306） |

## パスワード認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-password.md)/[拡張](wlan-group-password-adv.md) [^1]

## クライアント証明書認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-cert.md)/[拡張](wlan-group-cert-adv.md) [^1]

## Authentication Survivability

SingleIDでは、Aruba InstantのAuthentication Survivabilityを使用できません。詳しくは、[Authentication Survivabilityの利用について](authentication-survivability.md)を確認してください。

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * Aruba Instantでは固定のNAS-Identifierを設定できるため、拡張RADIUSサーバを利用した設定方法を推奨します。
