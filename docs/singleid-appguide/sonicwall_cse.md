# SonicWall Cloud Secure Edgeのアプリ連携
文書更新日:2025-03-14

## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、SonicWall Cloud Secure Edgeの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2025-01-18_19-06-37.png)](/images/2025-01-18_19-06-37.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。**シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2025-01-18_19-10-58.png)](/images/2025-01-18_19-10-58.png)

5.  以下の内容を設定し、**登録**ボタンをクリックして、アプリを登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **SSO 有効/無効** | **有効**を選択します。 |
    | **IdP エンドポイントメタデータ** | **ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、[SonicWall Cloud Secure EdgeのSAML認証設定](#sonicwall-cloud-secure-edgeのsaml認証設定)の手順を行う際に必要となります。 |
    | **SPのエンドポイントURL** | [SonicWall Cloud Secure EdgeのSAML認証設定](#sonicwall-cloud-secure-edgeのsaml認証設定)の手順の**Redirect URL**でコピーした内容を貼り付けます。 |


    [![Screenshot](/images/2025-01-18_19-16-43.png)](/images/2025-01-18_19-16-43.png)

### SonicWall Cloud Secure EdgeのSAML認証設定
1. SonicWall Cloud Secure EdgeのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. 右サイドメニューから**Settings**をクリックします。

    [![Screenshot](/images/2025-01-18_19-26-18.png)](/images/2025-01-18_19-26-18.png)

3. **Identity and Access＞End User**へ移動します。
4. 以下の内容で設定し、**SAVE**をクリックします。

    | **SonicWall Cloud Secure Edgeの設定項目** | **設定内容** |
    | :--- | :--- |
    | **User Identity Provider** ||
    | **Provider Name** | **Other**を選択し、**SingleID**と入力します。 |
    | **Provider Protocol** | **SAML**を選択します。 |
    | **Redirect URL** | 変更しないでください。このURLは、[SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順で使用します。 |
    | **Entity Issuer (optional)** | 空欄にします。 |
    | **IDP Settings** ||
    | **IDP Metadata Method** | Manual |
    | **IDP SSO Url** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **IDP CA Certificate** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストエディタに貼り付けます。そして、先頭行に、`-----BEGIN CERTIFICATE-----`の文字列をコピーして、貼り付け、最終行に、`-----END CERTIFICATE-----`の文字列をコピーして貼り付け、拡張子を.cerとして保存します。<br>そして、そのファイルをアップロードします。 |
    | **属性のマッピング** ||
    | **Username** | username |
    | **Email** | email |
    | **Groups** | groups |

    [![Screenshot](/images/2025-01-18_19-31-40.png)](/images/2025-01-18_19-31-40.png)

    [![Screenshot](/images/2025-01-18_19-44-41.png)](/images/2025-01-18_19-44-41.png)

## プロビジョニング
SonicWall Cloud Secure Edgeは、SAMLのJITプロビジョニングに対応しています。上記設定により、ユーザがSonicWall Cloud Secure Edgeへ始めてログインしたときに、ユーザが自動的に作成されます。その際、SingleIDのグループ情報もプロビジョニングされます。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
### SonicWall CSE Clientのダウンロード 
1. SonicWall Cloud Secure EdgeのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. 右サイドメニューから**Settings**をクリックします。

    [![Screenshot](/images/2025-01-18_19-26-18.png)](/images/2025-01-18_19-26-18.png)

3. **SonicWall CSE Client＞Deployment**へ移動します。
4. **CSE App for Windows**をクリックし、ダウンロードします。そして、インストール時に必要となる**Invite Code**をメモします。

    [![Screenshot](/images/2025-01-19_16-21-57.png)](/images/2025-01-19_16-21-57.png)

### SonicWall CSE Clientのインストール
1. ダウンロードした実行ファイルを起動して、インストールを開始します。
2. **REGISTER**をクリックします。

    [![Screenshot](/images/2025-01-19_16-27-21.png)](/images/2025-01-19_16-27-21.png)

3. [SonicWall CSE Clientのダウンロード](#sonicwall-cse-clientのダウンロード)の手順4でメモした**Invite Code**を入力し、**CONTINUE**をクリックします。

    [![Screenshot](/images/2025-01-19_16-27-44.png)](/images/2025-01-19_16-27-44.png)

4. ブラウザが起動して、SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されません。
   
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

5. SingleIDの認証に成功すると認証成功の画面が表示されます。

    [![Screenshot](/images/2025-01-19_16-28-25.png)](/images/2025-01-19_16-28-25.png)

6. デバイス登録を行います。デバイスの所有者を選択し、**CONTINUE**をクリックします。

    [![Screenshot](/images/2025-01-19_16-29-21.png)](/images/2025-01-19_16-29-21.png)

6. デバイスの登録が完了し、安全にアクセスが可能となります。

    [![Screenshot](/images/2025-01-19_16-29-47.png)](/images/2025-01-19_16-29-47.png)

