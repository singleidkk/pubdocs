# H3C WAシリーズの設定例一覧

## クラウドRADIUSと連携可能な製品の確認方法
[メーカーの製品ページ](https://www.h3c.com/jp/Products_and_Solutions/InterConnect/Wireless/){ target=_blank }を確認し、**ソフトウェアの仕様書**の**セキュリティポリシー/認証**の項目で、**802.1X認証**が記載されている製品です。

## パスワード認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-password.md)/[拡張](wlan-group-password-adv.md) [^1]

## クライアント証明書認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-cert.md)/[拡張](wlan-group-cert-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。
