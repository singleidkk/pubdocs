# @pocketのアプリ連携
## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、@pocketの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-06-05_16-03-46.png)](/images/2022-06-05_16-03-46.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-06-05_16-05-36.png)](/images/2022-06-05_16-05-36.png)

6. **SSO 有効/無効**を有効に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、@pocketの設定を行うときに必要となります。
8. **URL**に、@pocketのURLを設定します。
9.  **NameIDフォーマット(username または email)**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、usernameを入力し、SingleIDユーザのメールアドレスでログインする場合には、emailを入力します。  
10.  **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-06-05_16-07-36.png)](/images/2022-06-05_16-07-36.png)

### @pocketのSAML認証設定
1. @pocketへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/image.png)](/images/image.png)

2. 画面右上の設定アイコンをクリックして、プルダウンメニューの**管理者設定**を選択し、設定画面へ移動します。

    [![Screenshot](/images/image-2.png)](/images/image-2.png)

3. サイドメニューから**SAML認証設定**をクリックし、SAML設定画面へ移動します。
    
    [![Screenshot](/images/image-3.png)](/images/image-3.png)

4. 以下の内容で設定し、**更新**をクリックします。

    | **@pocketの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SAML認証の利用** | 有効 |
    | **SAML認証の利用を必須にする** | 無効<br>（有効にすると標準のユーザID、パスワードでのログインはできなくなります。SingleIDとSAMLで認証できることが確認できるまでは、有効にしないでください。）|
    | **IdPログインURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **IdPログアウトURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **IdPの公開鍵の証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストファイルとして保存します。そのファイルをアップロードします。 |

    [![Screenshot](/images/image-4-1024x455.png)](/images/image-4-1024x455.png)

## プロビジョニング
SingleIDから@pocketへのユーザおよびグループのプロビジョニングには、対応しておりません。

SingleIDおよび@pocketの両方に存在するユーザのみが、シングルサインオン可能です。同一のユーザおよびグループをSingleIDおよび@pocketへ登録してください。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. @pocketのログイン画面へアクセスします。
2. **外部IDログイン**ボタンが追加されていることを確認します。**外部IDログイン**をクリックします。（ここでは、ユーザIDおよびパスワードの入力は不要です。）
    
    [![Screenshot](/images/atpocket-5.png)](/images/atpocket-5.png)

3. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、@pocketへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

## ダイレクトログインの無効化
!!! danger
    以下は、ダイレクトログインを無効化する方法です。必ずSAML認証により**管理者権限**でログインできることを確認できてから、実施するようにしてください。SAML認証ができない状態で、ダイレクトログインを無効化すると@pocketへログインできなくなります。

**SAML認証設定**の設定画面の**SAML認証の利用を必須にする**を**有効**にします。
