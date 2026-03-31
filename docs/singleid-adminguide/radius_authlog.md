**SingleID 管理者ポータル＞ログ**画面では、以下のログに関する操作を行うことができます。

* RADIUS認証ログの表示/ダウンロード

## RADIUS認証ログ
### ログの表示

**ログ表示期間**を設定し、**表示**ボタンをクリックします。

!!! info
    少なくとも１か月分のログを表示可能です。

!!! warning
    RADIUSに設定したRADIUSクライアントのIPおよび共有シークレットが誤っている場合には、RADIUSリクエストがあってもRADIUS認証ログには表示されません。

!!! info
    RADIUS認証ログの表示には、認証実行後から5分程度時間がかかる場合があります。

### ログのダウンロード

**ログ表示期間**を設定し、**CSVダウンロード**または**EXCELダウンロード**ボタンをクリックします。

### 認証エラーのトラブルシューティング

#### `Rejected: There is no sites assosiated with sent NAS-IP-Address/NAS-Identifier.`

拡張RADIUSサーバを利用している場合に、**サイト識別する属性**または**属性値**が、実際にネットワーク機器から送信された**NAS-IP-Address**または**NAS-Identifier**と一致しない場合に発生します。

以下を確認してください。

* **認証＞RADIUS＞簡易設定**で登録したサイトの**サイト識別する属性**が適切か
* **属性値**が、実際に送信された**NAS-IP**または**NAS-ID**と一致しているか
* **NAS-Identifier**を利用している場合には、部分一致で吸収できる安定した共通文字列を設定できないか

詳細な確認方法については、[RADIUSクライアントのサイト識別する属性の確認方法](./radius.md#radiusクライアントのサイト識別する属性の確認方法)を参照してください。

#### `eap_peap: TLS Alert read:fatal:internal error`

Android11において、EAP方式が、PEAPまたはTLSの場合に、**ドメイン**への入力内容が正しくない場合に発生します。**ドメイン**には、**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**を入力します。

#### `eap_tls: (TLS) Alert read:fatal:unknown CA`

接続デバイス側で、RADIUSサーバ証明書の検証に使用するCAの設定が誤っている場合に発生します。RADIUSサーバ証明書は[Let's Encrypt](https://letsencrypt.org/certificates/){target=_blank}（サブジェクト: ISRG Root X1）のCAで発行されているため、接続デバイス側でも同CAで検証するよう設定してください。

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
