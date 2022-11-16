# Chatworkのアプリ連携
## シングルサインオン
* シングルサインオンが利用可能なプランは、エンタープライズプランです。詳しくは、[こちら](https://go.chatwork.com/ja/lp/remarketing_09/?adcode=onad_glsm_FV_money_original&utm_source=google&utm_medium=cpc&utm_campaign=brand&utm_term=chatwork&utm_content=602126140268&gclid=CjwKCAiAjs2bBhACEiwALTBWZfWlbO6UJ3S6MxFnsvcPOl6QR3EJ7rGbmt0nd5o8zspyer8Ai2V3FBoCneYQAvD_BwE#section-price){ target=_blank }を参照して下さい。

### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Chatworkの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-11-14_20-18-33.png)](/images/2022-11-14_20-18-33.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の英数字、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-11-14_20-21-07.png)](/images/2022-11-14_20-21-07.png)

6. **SSO 有効/無効**を有効に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Chatworkの設定を行うときに必要となります
8.   **NameIDフォーマット(username または email)**に、SAML NameIDフォーマットを指定します。SingleIDユーザのユーザ名でログインする場合には、**username**を入力し、SingleIDユーザのメールアドレスでログインする場合には、**email**を入力します。

    !!! info
        Chatworkのログインで使用するメールアドレスと同じメールアドレスが設定してあるSingleIDのパラメータを設定します。通常は、**email**です。

9.  **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-11-14_20-27-52.png)](/images/2022-11-14_20-27-52.png)

### ChatworkのSAML認証設定
1. Chatworkへ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-11-14_20-31-01.png)](/images/2022-11-14_20-31-01.png)

2. 右上の**ログインユーザ**をクリックし、**管理者設定**を選択します。

    [![Screenshot](/images/2022-11-14_14-21-54.png)](/images/2022-11-14_14-21-54.png)

3. サイドメニューの**ログイン制限**をクリックします。

    [![Screenshot](/images/2022-11-14_14-20-42.png)](/images/2022-11-14_14-20-42.png)

4. 以下の内容で設定します。

    | **Chatworkの設定項目** | **設定内容** |
    | :--- | :--- |
    | **専用ログインURL設定を行う** | :fontawesome-regular-square-check: |
    | **専用のログインURLのパス情報** | 任意の文字列 |
 
    [![Screenshot](/images/2022-11-14_14-19-06.png)](/images/2022-11-14_14-19-06.png)

5. **専用ログインURL**の**コピー**ボタンをクリックして、テキストエディター（メモ帳など）へ張り付けて、**専用のログインURL**をメモしておきます。

6. 以下の内容で設定します。

    | **Chatworkの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SAML認証によるシングルサインオン設定を行う** | :fontawesome-regular-square-check: |
    | **Identify ProviderのログインURL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`）<br><br> `/protocol/saml/clients/` を後ろに追加します。<br><br>[**SingleIDのアプリ連携設定**](#singleidのアプリ連携設定)の**手順4**で設定した**名前の文字列**を後ろに追加します。<br><br> 設定するログインURLは、環境や設定により異なりますが、`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml/clients/chatwork`のような形式となります。|
    | **Identify ProviderのエンティティID** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | **ログアウト後に遷移するURL** | **指定しない**を選択 |
    | **Identify Providerが署名に利用する公開鍵の証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、設定欄に貼り付けます。そして、先頭行に、`-----BEGIN CERTIFICATE-----`の文字列をコピーして、貼り付け、最終行に、`-----END CERTIFICATE-----`の文字列をコピーして貼り付けます。 |

    !!! info
        **ログアウト後に遷移するURL**の設定で、**指定する**を選択する場合には、**SingleIDのユーザポータル**のURLを指定することを推奨します。<br>
        Chatworkは、シングルログアウトをサポートしていないため、Chatworkをログアウトしても、SingleID連携している他のアプリはログアウトされません。SingleIDのユーザポータルの**セッション**画面から**全セッションからログアウト**ボタンをクリックすることで、**完全にログアウト**することができます。ユーザポータルのURLは、**Identify ProviderのエンティティID**で設定したURLに、`/account`の文字列を追加します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/account`）

    [![Screenshot](/images/2022-11-14_14-24-32.png)](/images/2022-11-14_14-24-32.png)

7. **SAML認証を利用しない管理者用ログインURL**の**コピー**ボタンをクリックして、テキストエディター（メモ帳など）へ張り付けて、**SAML認証を利用しない管理者用ログインURL**をメモしておきます。SAML認証の設定に問題があり、ログインできない場合に使用します。
8. **保存する**ボタンをクリックし、設定を保存します。

## プロビジョニング
SingleIDからChatworkへのユーザのプロビジョニングには、対応しておりません。

SingleIDおよびChatworkの両方に存在するユーザのみが、シングルサインオン可能です。メールアドレスを識別子としてシングルサインオンします。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
### ブラウザからのログイン
1. [**ChatworkのSAML認証設定**](#chatworkのsaml認証設定)の**手順5**でメモした**専用ログインURL**へアクセスします。
2. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Chatworkへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

### Chatworkのデスクトップ版アプリからのログイン
1. Chatworkのクライアントソフトウェアを起動します。右上のChatworkロゴをクリックします。

    [![Screenshot](/images/2022-11-16_11-12-27.png)](/images/2022-11-16_11-12-27.png)

2. [**ChatworkのSAML認証設定**](#chatworkのsaml認証設定)の**手順4**で設定した**専用ログインURLのパス**の文字列を入力し、**つづいてアカウント情報を入力する**ボタンをクリックします。

    [![Screenshot](/images/2022-11-16_11-20-40.png)](/images/2022-11-16_11-20-40.png)

3. SingleIDのログイン画面が表示されます。
    
    [![Screenshot](/images/2022-11-16_11-21-43.png)](/images/2022-11-16_11-21-43.png)