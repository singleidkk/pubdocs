# Salesforceのアプリ連携

## シングルサインオン
* **使用可能なインターフェース**: Salesforce Classic (使用できない組織もあります) および Lightning Experience の両方
* **認証プロバイダを使用可能なエディション**: Professional Edition、Enterprise Edition、Performance Edition、Unlimited Edition、および Developer Edition
* 事前に**私のドメイン**で**カスタムドメイン**の設定をしておく必要があります。
* 最新の設定手順は、[SAML シングルサインオンを使用するサービスプロバイダとして Salesforce を設定](https://developer.salesforce.com/docs/atlas.ja-jp.sso.meta/sso/sso_saml.htm)をご確認ください。

### SingleIDのアプリ登録およびIdP エンドポイントメタデータのダウンロード
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Salesforceの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-31-36.png)](/images/2022-08-16_3-31-36.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-08-16_3-34-24.png)](/images/2022-08-16_3-34-24.png)

6. **SSO 有効/無効**を**無効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Salesforceの設定を行うときに必要となります。
8. **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-08-16_3-35-22.png)](/images/2022-08-16_3-35-22.png)

!!! info
    URLおよび証明書は、SalesforceのSAML認証設定の後に設定します。

### SalesforceのSAML認証設定
1. Salesforceへ管理者権限を持つユーザでログインします。
2. ヘッダーの右側の:fontawesome-solid-gear:をクリックし、**設定**メニューを選択して設定画面へ移動します。

    [![Screenshot](/images/2022-08-16_4-36-00.png)](/images/2022-08-16_4-36-00.png)

3. サイドメニューの**ID＞シングルサインオン設定**をクリックします。
    
    [![Screenshot](/images/2022-08-16_4-50-15.png)](/images/2022-08-16_4-50-15.png)

4. **編集**ボタンをクリックし、**SAML を有効化**にチェックして、**保存**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_4-54-12.png)](/images/2022-08-16_4-54-12.png)

5. **メタデータファイルから新規作成**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_4-56-23.png)](/images/2022-08-16_4-56-23.png)

6. **メタデータファイルのファイル選択**で、SingleIDの**IdP エンドポイントメタデータ**ファイルを選択し、**作成**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_5-04-09.png)](/images/2022-08-16_5-04-09.png)

7. **SAML シングルサインオン構成**が**IdP エンドポイントメタデータ**により、自動入力されます。以下の内容で変更し、**保存**ボタンをクリックします。

    | **Salesforceの設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | SingleID |
    | **サービスプロバイダの起動要求バインド** | HTTPポストを選択 |
    | **シングルログアウト要求バインド** | HTTPポストを選択 |
    
    [![Screenshot](/images/2022-08-16_5-34-38.png)](/images/2022-08-16_5-34-38.png)

8. **メタデータのダウンロード**ボタンをクリックします。
 
    [![Screenshot](/images/2022-08-16_5-44-55.png)](/images/2022-08-16_5-44-55.png)

9. サイドメニューの**会社の設定＞私のドメイン**をクリックします。**認証設定**の**編集**ボタンをクリックします。

    [![Screenshot](/images/2022-08-16_6-30-45.png)](/images/2022-08-16_6-30-45.png)

10. **認証サービス**の**SingleID**にチェックを入れ**保存**ボタンをクリックします。

    [![Screenshot](/images/2022-08-16_6-21-58.png)](/images/2022-08-16_6-21-58.png)

    !!! warning
        **システム管理者**および**ユーザ**が、SingleIDで認証してSalesforceへログインできることを確認できるまでは、**ログインフォーム**のチェックを入れたままにするようにします。

### SingleIDのアプリ編集（URLおよび証明書の設定）
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録済みのSalesforceのアプリの横の**編集**ボタンをクリックします。**アプリ更新**画面がポップアップします。
3.  **シングルサインオン**タブに移動します。
7. 以下の内容を設定し、**更新**ボタンをクリックして、アプリを更新します。

    | **SingleIDの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SSO 有効/無効** | 有効 |
    | **URL** | Salesforceからダウンロードしたメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://d5i000009mrvseak.my.salesforce.com`） |
    | **証明書** | Salesforceからダウンロードしたメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDw……..`）をコピーして、入力します。 |

    [![Screenshot](/images/2022-08-16_5-57-20.png)](/images/2022-08-16_5-57-20.png)

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. Salesforceのログイン画面へアクセスします。
2. **SingleID**のリンクが追加されていることを確認します。
    
    [![Screenshot](/images/2022-08-16_6-32-15.png)](/images/2022-08-16_6-32-15.png)
    
3. **SingleID**リンクをクリックします。
4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Salesforceへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

## ダイレクトログインの無効化
!!! danger
    以下は、ダイレクトログインを無効化する方法です。**必ずSAML認証により管理者権限でログインできることを確認できてから、実施**するようにしてください。SAML認証ができない状態で、ダイレクトログインを無効化するとSalesforceへログインできなくなります。

1. Salesforceへ管理者権限を持つユーザでログインします。
2. ヘッダーの右側の:fontawesome-solid-gear:をクリックし、**設定**メニューを選択して設定画面へ移動します。

    [![Screenshot](/images/2022-08-16_4-36-00.png)](/images/2022-08-16_4-36-00.png)

3. サイドメニューの**会社の設定＞私のドメイン**をクリックします。**認証設定**の**編集**ボタンをクリックします。

    [![Screenshot](/images/2022-08-16_6-30-45.png)](/images/2022-08-16_6-30-45.png)

4.  **認証サービス**の**ログインフォーム**のチェックを外し**保存**ボタンをクリックします。

    [![Screenshot](/images/2022-08-16_7-12-38.png)](/images/2022-08-16_7-12-38.png)