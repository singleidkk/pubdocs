**SingleID 管理者ポータル＞ログ**画面では、以下のログに関する操作を行うことができます。

* RADIUS認証ログの表示/ダウンロード

## RADIUS認証ログ
### ログの表示

**ログ表示期間**を設定し、**表示**ボタンをクリックします。

!!! info
    少なくとも１か月分のログを表示可能です。

!!! warning
    RADIUSに設定したRADIUSクライアントのIPおよび共有シークレットが誤っている場合には、RADIUSリクエストがあってもRADIUS認証ログには表示されません。

### ログのダウンロード

**ログ表示期間**を設定し、**CSVダウンロード**または**EXCELダウンロード**ボタンをクリックします。

### 認証エラーのトラブルシューティング

#### `eap_peap: TLS Alert read:fatal:internal error`

Android11において、EAP方式が、PEAPまたはTLSの場合に、**ドメイン**への入力内容が正しくない場合に発生します。**ドメイン**には、**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**を入力します。

#### `No Auth-Type found: rejecting the user via Post-Auth-Type = Reject`

以下の場合に発生します。

* PAP認証を試み、そのユーザが存在しない場合
* ユーザが無効化されている場合

#### `pap: Cleartext password does not match "known good" password`

PAP認証を試み、そのユーザのパスワードが間違っている場合に発生します。

#### `chap: &control:Cleartext-Password is required for authentication`

以下の場合に発生します。

* CHAP認証を試み、そのユーザが存在しない場合
* ユーザが無効化されている場合

#### `chap: Password comparison failed: password is incorrect`

CHAP認証を試み、そのユーザのパスワードが間違っている場合に発生します。

#### `mschap: FAILED: No NT-Password. Cannot perform authentication`

以下の場合に発生します。

* MS-CHAP認証を試み、そのユーザが存在しない場合
* ユーザが無効化されている場合

#### `mschap: MS-CHAP2-Response is incorrect`

MS-CHAP認証を試み、そのユーザのパスワードが間違っている場合に発生します。

#### `eap_tls: Program returned code (2) and output 'error /var/run/radiusd/radiusd.client.XXXXXXXX: verification failed'`

クライアント証明書の検証に失敗した場合に発生します。以下の場合に、検証に失敗します。

* 適切な中間証明書で署名されていないクライアント証明書
* 失効されたクライアント証明書

#### `eap_tls: (TLS) OpenSSL says error 20 : unable to get local issuer certificate`

クライアント証明書の検証に失敗した場合に発生します。以下の場合に、検証に失敗します。

* 適切な中間証明書で署名されていないクライアント証明書
