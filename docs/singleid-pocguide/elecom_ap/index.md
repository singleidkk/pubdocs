# ELECOM 無線LANアクセスポイントの設定例一覧
文書更新日:2026-10-21

!!! warning
    本設定ガイドはレビュー中のドラフトです。記載内容は変更する場合があります。

## 検証環境

本設定ガイドは、以下の機器およびファームウェアで動作確認を行っています。
機種やファームウェアのバージョンが異なる場合、画面表示や設定項目が一部変わることがあります。

| **検証機種** | **ハードウェアバージョン** | **ファームウェアバージョン** |
| :--- | :--- | :--- |
| [WAB-BE187-M](https://www.elecom.co.jp/products/WAB-BE187-M.html){ target=_blank } | 1.0.0 | 2.0.13 |
| [WAB-BE36-M](https://www.elecom.co.jp/products/WAB-BE36-M.html){ target=_blank } | 1.0.0 | 2.0.13 |
| [WAB-BE36-S](https://www.elecom.co.jp/products/WAB-BE36-S.html){ target=_blank } | 1.0.0 | 2.0.13 |
| [WAB-BE72-M](https://www.elecom.co.jp/products/WAB-BE72-M.html){ target=_blank } | 1.0.0 | 2.0.13 |

WAB-BE187-Mで標準RADIUSおよび拡張RADIUSを使用した詳細な動作確認を行っています。ほかの3機種では、同じRADIUSおよびSSID設定機能を使用して、パスワード認証とクライアント証明書認証に成功することを確認しています。

!!! info
    * 認証試験は5GHz帯で実施しています。
    * 各機器のローカルWeb管理画面で設定しています。

!!! warning
    SingleIDは、WPA3 Enterprise 192-bitをサポートしていません。**WPAタイプ**には**WPA2/WPA3 Enterprise**を設定してください。

## パスワード認証

* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-password.md)/[拡張](wlan-group-password-adv.md) [^1]

## クライアント証明書認証

* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-cert.md)/[拡張](wlan-group-cert-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。

## 参考情報

* [WAB-BE187-M ユーザーズマニュアル](https://www.elecom.co.jp/support/manual/network/wireless-lan/ap/wab-be187-m/WAB-BE187-M_UM_v04.pdf){ target=_blank }
* [WAB-BE72-M/WAB-BE36-M/WAB-BE36-S ユーザーズマニュアル](https://www.elecom.co.jp/support/manual/network/wireless-lan/ap/wab-be36-s/WAB-BE36-S_UM_v03.pdf){ target=_blank }
