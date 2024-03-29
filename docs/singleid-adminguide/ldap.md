**SingleID 管理者ポータル＞認証＞LDAP**画面では、以下のLDAPに関する操作を行うことができます。

* クライアントの登録/編集/削除
* サービスアカウントの登録/編集/削除

## サポートする認証方式について
パスワード認証のみです。ワンタイムパスワード認証およびクライアント証明書認証はサポートしていません。

## 暗号化通信について
LDAPS(TCP636)をサポートしています。STARTTLS(TCP389)はサポートしていません。

## 基本情報
**SingleID 管理者ポータル＞認証＞LDAP＞基本情報**画面では、LDAPクライアントを設定するために必要な情報が記載されています。ここに記載されている情報を元に、LDAPクライアントとなるシステムの設定を行ってください。

## クライアント
**SingleID 管理者ポータル＞認証＞LDAP＞クライアント**画面では、接続可能なLDAPクライアントのグローバルIPアドレスを登録します。登録したグローバルIPアドレスを送信元とするLDAP通信のみがアクセス可能となります。初期状態では、どこからもアクセスすることができません。
!!! warning
    LDAPサーバのサービスアカウントは、登録された全ユーザの情報へアクセス可能のため、セキュリティの観点から、アクセス可能なデバイスは送信元のグローバルIPアドレスで制限してください。

## サービスアカウント
**SingleID 管理者ポータル＞認証＞LDAP＞サービスアカウント**画面では、LDAPサーバのディレクトリツリーにアクセス可能なアカウントを登録します。初期状態で、**ldap-bind-user**が登録されていますが、パスワードが設定されていません。**ldap-bind-user**を使ってLDAPサーバへ接続する際には、パスワードを設定してください。
