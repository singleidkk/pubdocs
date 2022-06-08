# Check Point Harmony Connect -Internet Access-のアプリ連携

## シングルサインオン
### SingleIDのアプリ連携設定
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. **アプリ登録**ボタンをクリックします。
    
    [![Screenshot](/images/image-1.png)](/images/image-1.png)

3. アプリカタログが表示されるので、Harmony Connectの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-06-06_19-59-14.png)](/images/2022-06-06_19-59-14.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-06-06_20-01-59.png)](/images/2022-06-06_20-01-59.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **URL**に、**https://cloudinfra-gw.portal.checkpoint.com/api/saml/sso**を設定します。
8. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Harmony Connectの設定を行うときに必要となります。
9. **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2022-06-06_20-02-58.png)](/images/2022-06-06_20-02-58.png)

### Harmony ConnectのSAML認証設定
1. [Harmony Connectの管理者ポータル](https://portal.checkpoint.com/signin)へ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-06-06_20-07-54.png)](/images/2022-06-06_20-07-54.png)

2. 画面左の**SETTINGS**アイコンをクリックし、設定画面へ移動します。
    
    [![Screenshot](/images/2022-06-06_20-12-11.png)](/images/2022-06-06_20-12-11.png)

3. サイドメニューの**Identity Provider**をクリックして、**Identitiy Provider**画面へ移動します。
4. **CONNECT NOW**ボタンをクリックします。
    
    [![Screenshot](/images/2022-06-06_21-17-10.png)](/images/2022-06-06_21-17-10.png)

5. **Generic SAML Provider**を選択し、**NEXT**ボタンをクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-21-24.png)](/images/2022-06-06_21-21-24.png)

6. 認証するユーザのメールドメインを管理する公開DNSサーバに、**Value**に表示されている値で、**TXTレコード**を登録します。そして、そのメールドメインを**Domain**に登録し、**NEXT**をクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-24-11.png)](/images/2022-06-06_21-24-11.png)

    !!! warning
        DNSでTXTレコードの確認ができるようになるまで、**Domain**にメールドメインの登録はできません。公開DNSサーバでTXTレコードのTTLが長く設定されていると、**Domain**へ登録可能となるまでにしばらく待つことになります。

7. **SELECT FILE**ボタンをクリックして、SingleIDのIdPエンドポイントメタデータのファイルを選択します。    
    
    [![Screenshot](/images/2022-06-06_21-38-18.png)](/images/2022-06-06_21-38-18.png)

8. メタデータが読み込まれ、**Service URL**が表示されます。**NEXT**ボタンをクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-39-32.png)](/images/2022-06-06_21-39-32.png)

9. **NEXT**ボタンをクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-41-27.png)](/images/2022-06-06_21-41-27.png)

10. **OK**ボタンをクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-42-01.png)](/images/2022-06-06_21-42-01.png)

    !!! info
        ユーザをアプリに登録していないので、ここでは、Identity Providerの接続テストを実施できません。設定が完了し、ユーザをアプリに登録してから接続テストを行います。

11. **ADD IDENTITIY PROVIDER**ボタンをクリックします。    
    
    [![Screenshot](/images/2022-06-06_21-43-13.png)](/images/2022-06-06_21-43-13.png)

12. 設定が完了したことを確認します。    
    
    [![Screenshot](/images/2022-06-06_21-44-48.png)](/images/2022-06-06_21-44-48.png)

## プロビジョニング
Harmony Connectクライアントから認証するメールアドレスをアクティベーションしたときに、Harmony Connectに自動的にユーザが作成されます。

ポリシー定義するためのユーザおよびグループのオブジェクトは自動で作成されないため、Harmony Connectの管理者ポータルから登録する必要があります。

## アプリにユーザ追加
1. SingleID 管理者ポータル＞認証＞アプリ連携画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. [Harmony Connectの管理者ポータル](https://portal.checkpoint.com/signin)へ管理者権限を持つユーザでログインします。
    
    [![Screenshot](/images/2022-06-06_20-07-54.png)](/images/2022-06-06_20-07-54.png)

2. 画面左の**SETTINGS**アイコンをクリックし、設定画面へ移動します。
    
    [![Screenshot](/images/2022-06-06_20-12-11.png)](/images/2022-06-06_20-12-11.png)

3. サイドメニューの**Identity Provider**をクリックして、**Identitiy Provider**画面へ移動します。

4. **Test Connectivity**ボタンをクリックし、**CONNECTIVITY TEST**画面を表示します。
    
    [![Screenshot](/images/2022-06-07_12-31-21.png)](/images/2022-06-07_12-31-21.png)
    
5. **RUN TEST**ボタンをクリックします。新しいブラウザのウィンドウがポッポアップします。
    
    [![Screenshot](/images/2022-06-07_12-33-05.png)](/images/2022-06-07_12-33-05.png)

6. SingleIDのログイン画面が表示されるので、**アプリに追加したユーザ**でログインします。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

7. **Connectivity Test Passed**となった場合には、Harmony ConnectのIdentity Providerの接続が正常に行われています。
    
    [![Screenshot](/images/2022-06-07_12-40-21.png)](/images/2022-06-07_12-40-21.png)

