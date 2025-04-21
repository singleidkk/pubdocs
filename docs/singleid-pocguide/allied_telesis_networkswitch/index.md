# アライドテレシス ネットワークスイッチの設定例一覧

## クラウドRADIUSと連携可能な製品の確認方法
アライドテレシス社のコマンドリファレンスを参照し、外部RADIUSと連携するコマンド（例: `radius-server`コマンド等）があることをご確認ください。

## パスワード認証
* 有線LANアクセス ユーザ/グループによるアクセス制限 [標準](./networkauth-group-password.md)/[拡張](./networkauth-group-password-adv.md) [^1]

## クライアント証明書認証
* 有線LANアクセス ユーザ/グループによるアクセス制限 [標準](./networkauth-group-cert.md)/[拡張](./networkauth-group-cert-adv.md) [^1]
  
[^1]:
  * 標準：標準RADIUSサーバを利用した設定方法です。
  * 拡張：拡張RADIUSサーバを利用した設定方法です。
  * 固定グローバルIPをもたないインターネット接続環境の場合には、拡張RADIUSサーバを利用した設定方法を参考にしてください。