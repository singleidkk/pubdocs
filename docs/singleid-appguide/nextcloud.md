# NextCloudのアプリ連携
## NextCloudのバージョン
* NextCloud 22, 23
* SSOとSAML認証 プラグイン：5.0.0

## シングルサインオン
### SingleIDのアプリ連携設定
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. **アプリ登録**ボタンをクリックします。
    [![Screenshot](/images/image-1.png)](/images/image-1.png)

3. アプリカタログが表示されるので、Nextcloudの**登録**ボタンをクリックします。
    [![Screenshot](/images/2022-06-04_20-51-08.png)](/images/2022-06-04_20-51-08.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    [![Screenshot](/images/image-2-1024x606.png)](/images/image-2-1024x606.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **URL**に、NextcloudのURLを設定します。
8. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Nextcloudの設定を行うときに必要となります。
9. **登録**ボタンをクリックして、アプリを登録します。
    [![Screenshot](/images/2022-06-04_20-57-43-1024x608.png)](/images/2022-06-04_20-57-43-1024x608.png)

### NextcloudのSAML認証設定
1. Nextcloudへ管理者権限を持つユーザでログインします。
    [![Screenshot](/images/2022-04-17_18-38-20.png)](/images/2022-04-17_18-38-20.png)

2. 画面右上のアイコンをクリックして、プルダウンメニューの**アプリ**を選択し、アプリ画面へ移動します。
    
    [![Screenshot](/images/2022-04-17_18-34-38.png)](/images/2022-04-17_18-34-38.png)

3. サイドメニューから**アクティブなアプリ**を選択し、**SSO & SAML authentication**がアクティブとなっていることを確認します。アクティブなアプリ一覧に表示されていない場合には、アプリをインストールをしてください。
    [![Screenshot](/images/2022-04-17_18-41-28.png)](/images/2022-04-17_18-41-28.png)

4. 画面右上のアイコンをクリックして、プルダウンメニューの**設定**を選択し、設定画面へ移動します。
    
    [![Screenshot](/images/2022-04-17_18-34-38-1.png)](/images/2022-04-17_18-34-38-1.png)

5. サイドメニューから**SSOとSAML認証**を選択し、**SSOとSAML認証**の設定画面へ移動します。
    [![Screenshot](/images/image-9.png)](/images/image-9.png)

6. 以下の内容で設定し、**更新**をクリックします。

    | **Nextcloudの設定項目** | **設定内容** |
    | :--- | :--- |
    | **グローバル設定** | ✅Nextcloud デスクトップクライアントにSAML認証を使用する（ユーザーの再認証が必要）<br>✅複数のユーザーのバックエンド（LDAPなど）の使用を許可する |
    | **一般** ||
    | **UIDをマップする属性** | username |
    | **IDプロバイダのオプションの表示名** | SingleID ログイン |
    | **Identity Providerデータ** ||
    | **IdPエンティティの識別子** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br>（例：https://auth-02-0001.poc.singleid.jp/auth/realms/90000013）|
    | **SPが認証要求メッセージを送信するIdPのターゲットURI** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br>（例：https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml） |
    | **SPがSLO要求を送信するIdPのURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br>（例：https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml） |
    | **IdPがSLOレスポンスを提供するURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br>（例：https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml） |
    | **IdPの公開鍵X.509証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..）をコピーして、入力します。 |
    | **属性のマッピング** ||
    | **表示名にマップする属性** | username |
    | **電子メールアドレスにマップする属性** | email |
    | **ユーザグループにマップする属性** | member |

## プロビジョニング
NextCloudは、SAMLのJITプロビジョニングに対応しています。上記設定により、NextCloudへユーザが自動的に作成されます。その際、SingleIDのグループ情報もプロビジョニングされます。

プロビジョニングを無効にしたい場合には、**SSOとSAML認証**の設定画面のグローバル設定にて、**ほかのバックエンドにアカウントが存在する場合のみ、認証を許可します。**を✅してください。

## アプリにユーザ追加
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. Nextcloudのログイン画面へアクセスします。
2. **SingleID ログイン**ボタンが追加されていることを確認します。
    [![Screenshot](/images/image-10.png)](/images/image-10.png)
    
3. **SingleID ログイン**ボタンをクリックします。
4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Nextcloudへログインできます。
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)
