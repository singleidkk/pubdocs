# WaWaOfficeのアプリ連携
## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、WaWaofficeの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-06-05_13-22-11.png)](/images/2022-06-05_13-22-11.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-06-05_13-23-10-1024x596.png)](/images/2022-06-05_13-23-10-1024x596.png)

6. **SSO 有効/無効**を有効に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、WaWaofficeの設定を行うときに必要となります。
8. **URL**に、WaWaofficeのURLを設定します。
9.  **NameIDフォーマット**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、usernameを入力し、SingleIDユーザのメールアドレスでログインする場合には、emailを入力します。  
10.  **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-06-05_13-25-25-1024x596.png)](/images/2022-06-05_13-25-25-1024x596.png)

### WaWaofficeのSAML認証設定
1. WaWaofficeへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-04-14_9-54-58.png)](/images/2022-04-14_9-54-58.png)

2. 画面右上のアイコンをクリックして、プルダウンメニューの**管理者設定**を選択し、設定画面へ移動します。

    [![Screenshot](/images/2022-04-14_9-59-58.png)](/images/2022-04-14_9-59-58.png)

3. **管理者システム設定**をクリックし、システム設定画面へ移動します。
    
    [![Screenshot](/images/image-8-1024x184.png)](/images/image-8-1024x184.png)

4. **外部サイトSAML認証設定**をクリックし、SAML設定画面へ移動します。
    
    [![Screenshot](/images/2022-04-14_10-04-55-1024x549.png)](/images/2022-04-14_10-04-55-1024x549.png)

5. 以下の内容で設定し、**更新**をクリックします。

    | **WaWaOfficeの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SAML認証利用設定** | 使用する |
    | **Identity Provider(IdP)設定** ||
    | **エンティティID** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | **ログインページURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **ログアウトページURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **IdPの公開鍵証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストファイルとして保存します。そのファイルをアップロードします。 |
    | **ログインIDがセットされている要素** | NameID要素 |

    [![Screenshot](/images/2022-04-14_10-17-29.png)](/images/2022-04-14_10-17-29.png)

## プロビジョニング
SingleIDからWaWaOfficeへのユーザおよびグループのプロビジョニングには、対応しておりません。

SingleIDおよびWaWaOfficeの両方に存在するユーザのみが、シングルサインオン可能です。同一のユーザおよびグループをSingleIDおよびWaWaofficeへ登録してください。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. WaWaOfficeのログイン画面へアクセスします。
2. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、WaWaOfficeへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

!!! warning
    WaWaOfficeのSAML認証を**使用する**にした場合、WaWaOfficeのログイン画面が出なくなり、SingleIDによるシングルサインオンのみでのログインに制限されます。もし、設定を間違えた等、WaWaOfficeのログイン画面からのログインが必要な場合は、URLの末尾にadminlogin.htmlを付けてアクセスしてください。

    例：https://asp11.wawa.ne.jp/demo-XwqPX1Eq/adminlogin.html

