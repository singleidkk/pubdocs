# Redmineのアプリ連携
## Redmineのバージョン
* Redmine 4.2.6.stable.21621
* redmine_openid_connectプラグイン 0.9.4

## シングルサインオン
### SingleIDのアプリ連携設定
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. **アプリ登録**ボタンをクリックします。
    
    [![Screenshot](/images/image-1.png)](/images/image-1.png)

3. アプリカタログが表示されるので、Redmineの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-06-07_20-45-11.png)](/images/2022-06-07_20-45-11.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    ![Screenshot](/images/2022-06-07_20-46-55.png)](/images/2022-06-07_20-46-55.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **URL**に、RedmineのURLを設定します。
8. **認証シークレット**に、Redmineと連携するための認証シークレット（任意の文字列）を設定します。この認証シークレットは、Redmineの設定を行うときに必要となります。
9. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Redmineの設定を行うときに必要となります。
10. **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-06-07_20-48-48.png)](/images/2022-06-07_20-48-48.png)

### RedmineのSAML認証設定
1. Redmineへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-06-07_21-10-11.png)](/images/2022-06-07_21-10-11.png)

2. ヘッダーメニューの**管理**をクリックして、管理メニューを表示します。
3. サイドメニューの**プラグイン**をクリックして、プラグイン一覧画面へ移動します。

    [![Screenshot](/images/2022-06-09_1-39-14.png)](/images/2022-06-09_1-39-14.png)

4. **Redmine Openid Connect plugin**の**設定**をクリックして、プラグインの設定画面へ移動します。
    
    [![Screenshot](/images/2022-06-09_1-41-15.png)](/images/2022-06-09_1-41-15.png)

5. 以下の内容で設定し、**適用**ボタンをクリックします。

    | **Redmineの設定項目** | **設定内容** |
    | :--- | :--- |
    | Enabled | :fontawesome-regular-square-check: |
    | **Client ID** | Redmineのログイン画面URLを指定します。<br><br>（例：https://redmine.example.com/） |
    | **OpenID Connect server url** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。issuerの値（URL）を入力します。<br><br>（例：https://auth-01-0001.dev.singleid.jp/auth/realms/90000013） |
    | **Client Secret** | SingleIDに設定した認証シークレットを入力します。 |
    | **OpenID Connect scopes** | openid,profile,email |
    | **Login Selector** | :fontawesome-regular-square-check: |
    | **Create user if not exists** | :fontawesome-regular-square-check: |
    
    [![Screenshot](/images/2022-06-09_1-47-34.png)](/images/2022-06-09_1-47-34.png)

## プロビジョニング
Redmine Openid Connect pluginは、OIDCのJITプロビジョニングに対応しています。上記設定により、Redmineへユーザが自動的に作成されます。

!!! warning
    SingleIDのグループ情報はプロビジョニングされません。

**Redmineのシステム管理者の権限をSingleIDのグループのメンバーに付与したい場合**には、**OpenID Connect Configuration**の設定画面にて、権限を付与したいSingleIDのグループの名前を**Admins group (members of this group are treated as admin)**へ設定してください。

**プロビジョニングを無効にしたい場合**には、**OpenID Connect Configuration**の設定画面にて、**Create user if not exists**を無効にしてください。

## アプリにユーザ追加
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. Redmineのログイン画面へアクセスします。
2. **Login with SSO**ボタンが追加されていることを確認します。
    
    [![Screenshot](/images/2022-06-09_2-17-19.png)](/images/2022-06-09_2-17-19.png)
    
3. **Login with SSO**ボタンをクリックします。
4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Redmineへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)
