# Pleasanterのアプリ連携

Pleasanterのシングルサインオン設定の最新情報は、[プリザンターでSAML認証を利用する](https://www.pleasanter.org/manual/saml){target=_blank}を参照してください。

## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Pleasanterの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-12-10_23-43-41.png)](/images/2022-12-10_23-43-41.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-12-10_23-47-26.png)](/images/2022-12-10_23-47-26.png)

6. **SSO 有効/無効**を有効に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Pleasanterの設定を行うときに必要となります。
8. **URL**に、PleasanterのURLを設定します。（例：https://pleasanter.example.com/pleasanter/）
9.  **NameIDフォーマット**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、usernameを入力し、SingleIDユーザのメールアドレスでログインする場合には、emailを入力します。  
10.  **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-12-10_23-48-58.png)](/images/2022-12-10_23-48-58.png)

### PleasanterのSAML認証設定
Pleasanterのサーバで作業を行います。

1. SingleIDの公開鍵証明書をインポートします。SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストファイル（テキストファイルの拡張子はcrt）として保存します。
2. 保存したファイルをダブルクリックします。証明書ウィンドウが表示されます。
3. **証明書をインストール**ボタンをクリックし、**保存場所をローカルコンピューター**、**証明書ストアを個人**として、証明書をインストールします。
4. Authentication.json（標準構成で、`C:¥web¥pleasanter¥Implem.Pleasanter¥App_Data¥Parameters`ディレクトリに配置されています。）を編集します。

5. 以下の内容で設定します。

    | **Pleasanterの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Provider** | SAML |
    | **SamlTenantId** | 1 |
    | **SPOptions** ||
    | **EntityId** | [SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順8で入力した**URL**の後ろに、**Saml2**の文字列を付加します。 |
    | **ReturnUrl** | [SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順8で入力した**URL**の後ろに、**Users/SamlLogin**の文字列を付加します。 |
    | **AuthenticateRequestSigningBehavior** | IfIdpWantAuthnRequestsSigned |
    | **OutboundSigningAlgorithm** | http://www.w3.org/2001/04/xmldsig-more#rsa-sha256 |
    | **MinIncomingSigningAlgorithm** | http://www.w3.org/2001/04/xmldsig-more#rsa-sha256 |
    | **DisableOverwriteName** | true |
    | **IgnoreMissingInResponseTo** | false |
    | **PublicOrigin** | null |
    | **ServiceCertificates** | [] |
    | **IdentityProviders** || 
    | **EntityId** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | **SignOnUrl** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **LogoutUrl** | null |
    | **AllowUnsolicitedAuthnResponse** | true |
    | **Binding** | HttpPost |
    | **WantAuthnRequestsSigned** | false |
    | **DisableOutboundLogoutRequests** | true |
    | **LoadMetadata** | false|
    | **MetadataLocation** | null |
    | **SigningCertificate** ||
    | **StoreName** | My |
    | **StoreLocation** | LocalMachine |
    | **X509FindType** | FindByThumbprint |
    | **FindValue** | 証明書の検索値（拇印）は以下の手順で確認できます。<br>1. [Windows]キー+[R]を押下し、ファイル名を指定して実行ダイアログに「certlm.msc」を入力して「OK」<br>2. 管理コンソールの「個人」-「証明書」から対象の証明書をダブルクリック（または右クリックで「開く」）<br>3. 証明書情報画面の「詳細」タブ-「拇印」フィールドの「値」に表示される文字列 |

    ``` json title="Authentication.json 設定例(SAML認証に関連する項目のみ抜粋"
    {
        "Provider": "SAML",
        // ～中略～
        "SamlParameters": {
            "Attributes": {
                "Name": "Name",
                "UserCode": "UserCode",
                "Birthday": "Birthday",
                "Gender": "Gender",
                "Language": "Language",
                "TimeZone": "TimeZone",
                "TenantManager": "TenantManager",
                "DeptCode": "DeptCode",
                "Dept": "Dept",
                "Body": "Body",
                "MailAddress": "{NameId}"
            },
            "SamlTenantId": 1,
            "SPOptions": {
                "EntityId": "https://pleasanter.example.com/pleasanter/Saml2",
                "ReturnUrl": "https://pleasanter.example.com/pleasanter/Users/SamlLogin",
                "AuthenticateRequestSigningBehavior": "IfIdpWantAuthnRequestsSigned",
                "OutboundSigningAlgorithm": "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256",
                "MinIncomingSigningAlgorithm": "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256",
                "DisableOverwriteName": true,
                "IgnoreMissingInResponseTo": false,
                "PublicOrigin": null,
                "ServiceCertificates": []
            },
            "IdentityProviders": [
                {
                    "EntityId": "https://auth-02-0001.poc.singleid.jp/auth/realms/90000013",
                    "SignOnUrl": "https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml",
                    "LogoutUrl": null,
                    "AllowUnsolicitedAuthnResponse": true,
                    "Binding": "HttpPost",
                    "WantAuthnRequestsSigned": false,
                    "DisableOutboundLogoutRequests": true,
                    "LoadMetadata": false,
                    "MetadataLocation": null,
                    "SigningCertificate": {
                        "StoreName": "My",
                        "StoreLocation": "LocalMachine",
                        "X509FindType": "FindByThumbprint",
                        "FindValue": "50B459426DE554010B35E9XXXXXXXXXXXXXXXXX"
                    }
               }
            ]
        }
    }
    ```

6. IISを再起動します。

## プロビジョニング
SingleIDからPleasanterへのユーザおよびグループのプロビジョニングには、対応しておりません。

SingleIDおよびPleasanterの両方に存在するユーザのみが、シングルサインオン可能です。同一のユーザおよびグループをSingleIDおよびPleasanterへ登録してください。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. Pleasanterのログイン画面へアクセスします。
2. ログイン画面にある**SSOログイン**ボタンをクリックします。
3. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Pleasanterへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)
