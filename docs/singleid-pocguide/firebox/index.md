# Fireboxの設定例一覧
## パスワード認証
* [リモートアクセスVPN(Mobile VPN with IKEv2) グループによるアクセス制限](vpn-group-password.md)
* [リモートアクセスVPN(Mobile VPN with SSL) グループによるアクセス制限](sslvpn-group-password.md)
* [無線LANアクセス グループによるアクセス制限](wlan-group-password.md)
* [無線LANアクセス グループごとに、接続可能な無線LANアクセスポイントをSSIDで制限](wlan-group-ssid-password.md)
* [管理者ログイン](adminlogin-user-password.md)

## クライアント証明書認証
* [無線LANアクセス グループによるアクセス制限](wlan-group-cert.md)
* [無線LANアクセス グループごとに、接続可能な無線LANアクセスポイントをSSIDで制限](wlan-group-ssid-cert.md)

## ２要素認証（パスワード認証＋ワンタイムパスワード認証）[^1]
* [リモートアクセスVPN(Mobile VPN with IKEv2) グループによるアクセス制限](vpn-group-otp.md)
* [リモートアクセスVPN(Mobile VPN with SSL) グループによるアクセス制限](sslvpn-group-otp.md)
* [管理者ログイン](adminlogin-user-otp.md)

[^1]:
  * 一般的に、無線LANアクセス時に、ワンタイムパスワード認証は使用することがないことから、SingleIDでは、無線LANアクセス時のワンタイムパスワードをサポートしておりません。パスワード認証またはクライアント証明書による認証となります。
  * ２要素認証（パスワード認証＋ワンタイムパスワード）は、**PAP**または**EAP-TLS-PAP**認証時のみ可能となります。その他のパスワード入力タイプの認証方式（CHAP、MSCHAP、PEAP）の場合には、ワンタイムパスワードは利用できません。
  * **SingleIDへソフトウェアトークンの登録**を行っていないユーザは、ワンタイムパスワードは無効となり、パスワードでの認証となります。