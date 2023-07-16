# Google Workspaceのアプリ連携
## シングルサインオン

### SingleIDのアプリ登録およびIdPエンドポイントメタデータのダウンロード
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Google Workspaceの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2023-07-16_0-57-34.png)](/images/2023-07-16_0-57-34.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2023-07-16_1-09-08.png)](/images/2023-07-16_1-09-08.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Google Workspaceの設定を行うときに必要となります。
8. 以下の内容で設定し、**登録**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **パスワード認証** | パスワード認証の有効/無効の設定です。 |
    | **証明書認証** | 証明書認証の有効/無効の設定です。 |
    | **SSO Cookie認証** | Cookie認証の有効/無効の設定です。 |
    | **NameIDフォーマット** | email |
    
    !!! info
        * **パスワード認証：有効、証明書認証：有効の場合**、ログイン時に、**パスワード認証または証明書認証の認証方式の選択が可能**です。証明書がデバイスにインストールされていない場合には、自動的に、パスワード認証方式が選択されます。
        * **パスワード認証：無効、証明書認証：有効の場合**、ログイン時に、**証明書認証のみが可能**です。証明書がインストールされていないデバイスからのログインはできません。
        * **SSO Cookie認証：無効の場合**、他のアプリで認証済みであっても、認証が要求されます。つまり、SSOではなくなります。

    [![Screenshot](/images/2023-07-16_1-14-39.png)](/images/2023-07-16_1-14-39.png)

### Google WorkspaceのSAML認証設定
1. [Google Workspace 管理コンソール](https://admin.google.com/){ target=_blank }へログインします。
2. 左サイドメニューから**セキュリティ＞認証＞サードパーティのIdPによるSSO**を選択して**サードパーティのIdPによるSSO**設定画面へ移動します。
3. **組織向けのサードパティのSSOプロファイル**セクションの**SSOプロファイルを追加**をクリックし、SSOプロファイルを設定します。

    [![Screenshot](/images/2023-07-16_11-14-16.png)](/images/2023-07-16_11-14-16.png)

4. 以下の内容で設定し、**保存**をクリックします。

    | **Google Workspaceの設定項目** | **設定内容** |
    | :--- | :--- |
    | **サードパーティの ID プロバイダで SSO を設定する** | 有効 |
    | **ログインページの URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **ログアウト ページの URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを参照し、以下のように入力します。<br><br>`<entityIDの値>/protocol/openid-connect/logout?redirect_uri=<entityIDの値>/account/`<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/openid-connect/logout?redirect_uri=https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/account/`）|
    | **ドメイン固有の発行元を使用** | 使用しない |
    | **パスワード変更用 URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを参照し、以下のように入力します。<br><br>`<entityIDの値>/account/`<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/account/`）|
    | **確認用の証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、`-----BEGIN CERTIFICATE-----`と`-----END CERTIFICATE-----`のテキストの間に張り付けて、テキストファイル（PEM形式の証明書）として保存します。そのファイルをアップロードします。 |

    [![Screenshot](/images/2023-07-16_11-23-55.png)](/images/2023-07-16_11-23-55.png)

## プロビジョニング

SingleIDおよびGoogle Workspaceの両方に存在するユーザのみが、シングルサインオン可能です。**SingleIDのメールアドレス**と**Google Workspaceのメールアドレス**が一致する必要があります。

[Google Cloud Directory Sync(GCDS)によるユーザ同期](gcds.md)を行うことで、SingleIDのユーザをGoogle Workspaceへ同期することができます。

!!! warning
    クラウドLDAPが、Google Workspaceと同期できるのはユーザ情報のみです。グループ同期はできません。
    
## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. [Google](https://www.google.com){ target=_blank }へアクセスします。
2. **ログイン**ボタンをクリックします。
3. 特権管理者の権限を持たないユーザの**メールアドレス**を入力し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/2023-07-16_12-38-53.png)](/images/2023-07-16_12-38-53.png)

4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Google Workspaceへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)
