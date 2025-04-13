# アライドテレシス 無線LANアクセスポイントの設定例一覧

ウェブ管理画面で設定可能なAT-TQおよびAT-TQmシリーズの設定例です。AT-TQRシリーズの設定については、コマンドラインからの設定となります。

## クラウドRADIUSと連携可能な製品の確認方法
[無線LANアクセスポイント 仕様一覧表](https://www.allied-telesis.co.jp/web-material/products/wireless_ap.pdf)を確認し、**IEEE 802.1X**および**multi SSID/VLAN/ダイナミックVLAN**が利用可能な製品です。

## パスワード認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-password.md)/[拡張](wlan-group-password-adv.md) [^1]

## クライアント証明書認証
* 無線LANアクセス ユーザ/グループによるアクセス制限 [標準](wlan-group-cert.md)/[拡張](wlan-group-cert-adv.md) [^1]
  
[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。