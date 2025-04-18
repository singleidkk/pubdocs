# FortiGateの設定例一覧
## パスワード認証
* リモートアクセスVPN(IPSec) グループによるアクセス制限 [標準](vpn-group-password_new2.md)/[拡張](vpn-group-password_new2-adv.md) [^1]
* 管理者ログイン グループによるアクセス制限 [標準](adminlogin-group-password_new2.md)/[拡張](adminlogin-group-password_new2-adv.md) [^1]

## クライアント証明書認証
* [リモートアクセスVPN(IPSec)](vpn-cert.md)

## ２要素認証（パスワード認証＋ワンタイムパスワード認証）
* リモートアクセスVPN(IPSec) グループによるアクセス制限 [標準](vpn-group-otp_new2.md)/[拡張](vpn-group-otp_new2-adv.md) [^1]
* 管理者ログイン グループによるアクセス制限 [標準](adminlogin-group-otp_new2.md)/[拡張](adminlogin-group-otp_new2-adv.md) [^1]

## ２要素認証（パスワード＋クライアント証明書認証）
* リモートアクセスVPN(IPSec) グループによるアクセス制限 [標準](vpn-group-password-cert.md)/[拡張](vpn-group-password-cert-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。
