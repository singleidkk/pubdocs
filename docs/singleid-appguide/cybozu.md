# サイボウズのアプリ連携
## シングルサインオン
### SingleIDのアプリ連携設定
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. **アプリ登録**ボタンをクリックします。
    
    [![Screenshot](/images/image-1.png)](/images/image-1.png)

3. アプリカタログが表示されるので、サイボウズの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-06_19-31-48.png)](/images/2022-08-06_19-31-48.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-08-06_19-34-17.png)](/images/2022-08-06_19-34-17.png)

6. **SSO 有効/無効**を有効に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、サイボウズの設定を行うときに必要となります。
8. **URL**に、サイボウズのURLを設定します。
9.   **NameIDフォーマット(username または email)**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、usernameを入力し、SingleIDユーザのメールアドレスでログインする場合には、emailを入力します。

10. **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-08-06_19-39-25.png)](/images/2022-08-06_19-39-25.png)

### サイボウズのSAML認証設定
1. サイボウズの管理ポータルへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-08-06_19-48-09.png)](/images/2022-08-06_19-48-09.png)

2. サイボウズの管理ポータル＞Cybozu.com 共通管理＞システム管理＞ユーザー管理＞セキュリティ＞ログインへ移動します。

    [![Screenshot](/images/2022-08-06_19-57-49.png)](/images/2022-08-06_19-57-49.png)


3. 以下の内容で設定し、**更新**をクリックします。

    | **サイボウズの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SAML認証を有効にする** | :fontawesome-regular-square-check: |
    | **Identity ProviderのSSOエンドポイントURL（HTTP-Redirect）** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **cybozu.comからのログアウト後に遷移するURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLに/accountを末尾に追加したURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/account`） |    
    | **Identity Providerが署名に使用する公開鍵の証明書使用している証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、`-----BEGIN CERTIFICATE-----`と`-----END CERTIFICATE-----`のテキストの間に張り付けて、テキストファイル（PEM形式の証明書）として保存します。そのファイルをアップロードします。 |

    [![Screenshot](/images/2022-08-06_20-02-33.png)](/images/2022-08-06_20-02-33.png)

## プロビジョニング
SingleIDからサイボウズへのユーザのプロビジョニングには、対応しておりません。

SingleIDおよびサイボウズの両方に存在するユーザのみが、シングルサインオン可能です。同一のユーザをSingleIDおよびサイボウズへ登録してください。

## アプリにユーザ追加
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
### ブラウザからのログイン
1. サイボウズの管理ポータルへアクセスします。
2. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、サイボウズへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

## ダイレクトログインの無効化
サイボウズの管理ポータル＞Cybozu.com 共通管理＞システム管理＞ユーザー管理＞セキュリティ＞ログイン画面の**SAML認証の使用を必須にする**を**有効**にします。

!!! info
    詳しくは、以下を参照ください。
    https://jp.cybozu.help/general/ja/admin/list_saml/saml_restriction.html