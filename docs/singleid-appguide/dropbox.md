# Dropboxのアプリ連携
## シングルサインオン
* Advancedプランまたは Enterpriseプランに加入している Dropboxの管理者は、シングル サインオン（SSO）を有効にできます。詳しくは、[こちら](https://help.dropbox.com/ja-jp/security/sso-admin){ target=_blank }を参照して下さい。
* SingleIDと連携可能なDropboxのテナントは１つのみです。

### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Dropboxの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2024-02-14_14-28-36.png)](/images/2024-02-14_14-28-36.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2024-02-14_14-31-40.png)](/images/2024-02-14_14-31-40.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、[DropboxのSAML認証設定](#dropboxのsaml認証設定)を行うときに必要となります。
8. **URL**に、[DropboxのSAML認証設定](#dropboxのsaml認証設定)の手順5の**SSO ログインの URL**でコピーした内容を貼り付けます。
9.  **NameIDフォーマット**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、**username**を入力し、SingleIDユーザのメールアドレスでログインする場合には、**email**を入力します。

    !!! info
        Dropboxのログインで使用するユーザ名は、メールアドレスです。SingleIDのユーザ名がメールアドレスでない場合には、**username**ではなく、**email**を設定します。

10. **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2024-02-14_18-57-01.png)](/images/2024-02-14_18-57-01.png)

### DropboxのSAML認証設定
1. Dropboxへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2024-02-14_16-04-13.png)](/images/2024-02-14_16-04-13.png)

2. メニューの**管理コンソール**をクリックします。

    [![Screenshot](/images/2024-02-12_18-37-55.png)](/images/2024-02-12_18-37-55.png)

3. メニューの**設定**をクリックします。

    [![Screenshot](/images/2024-02-12_18-38-51.png)](/images/2024-02-12_18-38-51.png)

4. 認証の**シングル サインオン**をクリックします。

    [![Screenshot](/images/2024-02-12_18-39-30.png)](/images/2024-02-12_18-39-30.png)

5. 以下の内容で設定します。

    | **Dropboxの設定項目** | **設定内容** |
    | :--- | :--- |
    | **シングル サインオン** | **任意**または**必須**<br><br>**必須**を選択すると、ユーザはSSOでしかログインできなくなります。ユーザが正常にシングルサインオンできることを確認し、ユーザーへの周知が完了してから必須を選択することを推奨します。 |
    | **アイデンティティ プロバイダのログイン URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **アイデンティティ プロバイダのログアウト URL（オプション）** | SingleIDのユーザポータルのURLを設定することを推奨します。ユーザポータルのURLは、SingleIDの**エンティティID**に、`/account`の文字列を追加します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/account`<br><br>Dropboxは、シングルログアウトをサポートしていないため、Dropboxをログアウトしても、SingleID連携している他のアプリはログアウトされません。SingleIDのユーザポータルの**セッション**画面から**全セッションからログアウト**ボタンをクリックすることで、**完全にログアウト**することができます。<br><br>SingleIDの**エンティティID**を確認するには、SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLが**エンティティID**です。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | **X.509 証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストエディタに貼り付けます。そして、先頭行に、`-----BEGIN CERTIFICATE-----`の文字列をコピーして、貼り付け、最終行に、`-----END CERTIFICATE-----`の文字列をコピーして貼り付け、テキストファイルとして保存します。<br>そして、そのファイルをアップロードします。 |
    | **SSO ログインの URL** | **リンクをコピー**ボタンをクリックし、[SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順8にあるように**URL**に貼り付けます。 |
 
    [![Screenshot](/images/2024-02-12_18-40-03.png)](/images/2024-02-12_18-40-03.png)

6. **保存**ボタンをクリックします。

## プロビジョニング
SingleIDからDropboxへのユーザのプロビジョニングには、対応しておりません。

SingleIDおよびDropboxの両方に存在するユーザのみが、シングルサインオン可能です。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
### ブラウザからのログイン
1. Dropboxの[ログイン画面](https://www.dropbox.com/login){ target=_blank }へアクセスします。
2. メールアドレスを入力し、**続行**ボタンをクリックします。
 
    [![Screenshot](/images/2024-02-14_16-04-13.png)](/images/2024-02-14_16-04-13.png)

3. **シングル サインオンで続行**ボタンでをクリックします。

    [![Screenshot](/images/2024-02-14_17-42-30.png)](/images/2024-02-14_17-42-30.png)

4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Dropboxへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)
