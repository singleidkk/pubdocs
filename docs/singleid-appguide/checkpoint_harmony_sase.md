# Check Point Harmony SASEのアプリ連携
文書更新日:2025-04-23

## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Check Point Harmony SASEの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2025-04-23_10-00-41.png)](/images/2025-04-23_10-00-41.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。**シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2025-04-23_10-06-03.png)](/images/2025-04-23_10-06-03.png)

5.  以下の内容を設定し、**登録**ボタンをクリックして、アプリを登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **SSO 有効/無効** | **有効**を選択します。 |
    | **IdP エンドポイントメタデータ** | **ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、[Check Point Harmony SASEのSAML認証設定](#check-point-harmony-saseのsaml認証設定)の手順を行う際に必要となります。 |
    | **SPのエンティティID** | **米国ベースのプラットフォームの場合**:  `urn:auth0:perimeter81:{{WORKSPACE}}-oc`<br>**EUベースのプラットフォームの場合**: `urn:auth0:eu-sase-checkpoint:{{WORKSPACE}}-oc`<br><br>{{WORKSPACE}}を実際のワークスペース名に置き換えてください。 |
    | **SPのエンドポイントURL** | **米国ベースのプラットフォームの場合**: `https://auth.perimeter81.com/login/callback?connection={{WORKSPACE}}-oc`<br>**EUベースのプラットフォームの場合**: `https://auth.eu.sase.checkpoint.com/login/callback?connection={{WORKSPACE}}-oc`<br><br>{{WORKSPACE}}を実際のワークスペース名に置き換えてください。 |

    [![Screenshot](/images/2025-04-23_10-08-20.png)](/images/2025-04-23_10-08-20.png)

### Check Point Harmony SASEのSAML認証設定
1. Check Point Harmony SASEのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. **Settings＞Identity Providers**画面へ移動し、**Add Provider**ボタンをクリックします。

    [![Screenshot](/images/2025-04-21_20-36-07.png)](/images/2025-04-21_20-36-07.png)

2. **SAML2.0 Identity Providers**を選択し、**Continue**ボタンをクリックします。

    [![Screenshot](/images/2025-04-21_20-36-57.png)](/images/2025-04-21_20-36-57.png)

4. 以下の内容で設定し、**Done**ボタンをクリックします。

    | **Check Point Harmony SASEの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Sign in URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **Domain Aliases** | 自組織のドメイン名をFQDNで設定します。（例: singleid.jp） |
    | **X509 Signing Certificate** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストエディタに貼り付けます。そして、先頭行に、`-----BEGIN CERTIFICATE-----`の文字列をコピーして貼り付け、最終行に、`-----END CERTIFICATE-----`の文字列をコピーして貼り付け、拡張子を.pemとして保存します。<br>そして、そのファイルをアップロードします。 |

    [![Screenshot](/images/2025-04-23_10-46-29.png)](/images/2025-04-23_10-46-29.png)

## プロビジョニング
Check Point Harmony SASEは、SAMLのJITプロビジョニングに対応しています。上記設定により、ユーザがCheck Point Harmony SASEへ始めてログインしたときに、ユーザが自動的に作成されます。その際、SingleIDのグループ情報もプロビジョニングされます。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
### Check Point Harmony SASE agentのダウンロード 
1. Check Point Harmony SASEのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. **Devices＞Downloads**画面へ移動します。Windows（EXE）の**Download**ボタンをクリックし、ダウンロードします。

    [![Screenshot](/images/2025-04-23_13-51-53.png)](/images/2025-04-23_13-51-53.png)

### Check Point Harmony SASE agentのインストール
1. ダウンロードした実行ファイルを起動して、インストールを開始します。
2. インストールが完了するとタスクトレイに**Harmony SASE**アイコンが表示されるので、クリックします。その後、**Sign In**ボタンをクリックします。

    [![Screenshot](/images/2025-04-23_14-04-00.png)](/images/2025-04-23_14-04-00.png)

3. ブラウザが起動し、ログインURLを入力する画面が表示されます。Check Point Harmony SASEのログインURLを入力し、**Continue**ボタンをクリックします。

    [![Screenshot](/images/2025-04-23_14-18-11.png)](/images/2025-04-23_14-18-11.png)

3. Check Point Harmony SASEのログインが画面が表示されます。**Sign in with SSO**ボタンをクリックします。

    [![Screenshot](/images/2025-04-23_14-21-44.png)](/images/2025-04-23_14-21-44.png)

4. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されません。
   
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

5. SingleIDの認証に成功するとCheck Point Harmony SASEのデスクトップアプリが起動します。

    [![Screenshot](/images/2025-04-23_14-27-55.png)](/images/2025-04-23_14-27-55.png)
