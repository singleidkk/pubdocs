# YAMAHA RTX830の設定例一覧
## パスワード認証
* リモートアクセスVPN ユーザ/グループによるアクセス制限 [標準](vpn-group-password.md)/[拡張](vpn-group-password-adv.md) [^1]
* 管理者ログイン ユーザ/グループによるアクセス制限 [標準](adminlogin-group-password.md)/[拡張](adminlogin-group-password-adv.md) [^1]

## 2要素認証（パスワード認証＋ワンタイムパスワード認証）
* リモートアクセスVPN ユーザ/グループによるアクセス制限 [標準](vpn-group-otp.md)/[拡張](vpn-group-otp-adv.md) [^1]
* 管理者ログイン ユーザ/グループによるアクセス制限 [標準](adminlogin-group-otp.md)/[拡張](adminlogin-group-otp-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。