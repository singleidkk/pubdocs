# Anti Spreader セキュアスイッチの設定例一覧
## パスワード認証
* 有線LANアクセス ユーザ/グループによるアクセス制限 [標準](./networkauth-group-password.md)/[拡張](./networkauth-group-password-adv.md) [^1]

## クライアント証明書認証
* 有線LANアクセス ユーザ/グループによるアクセス制限 [標準](./networkauth-group-cert.md)/[拡張](./networkauth-group-cert-adv.md) [^1]

[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。