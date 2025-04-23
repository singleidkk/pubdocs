# Check Point Quantum Sparkのアプリ連携

## シングルサインオン
### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Check Point Quantum Sparkの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2025-02-15_18-01-47.png)](/images/2025-02-15_18-01-47.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2025-02-15_18-02-20.png)](/images/2025-02-15_18-02-20.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、[Check Point Quantum SparkのSAML認証設定](#check-point-quantum-sparkのsaml認証設定)を行うときに必要となります。
8. **SPのエンティティID**に、[Check Point Quantum SparkのSAML認証設定](#check-point-quantum-sparkのsaml認証設定)の手順3の**一意の識別子 URL**でコピーした内容を貼り付けます。
9. **SPのエンドポイントURL**に、[Check Point Quantum SparkのSAML認証設定](#check-point-quantum-sparkのsaml認証設定)の手順3の**返信 URL**でコピーした内容を貼り付けます。
10.  **登録**ボタンをクリックして、アプリを登録します。
    
    [![Screenshot](/images/2025-02-15_18-03-00.png)](/images/2025-02-15_18-03-00.png)

### Check Point Quantum SparkのSAML認証設定
1. **Check Point Quantum Spark GUI＞VPN＞認証サーバ**画面へ移動します。
2. **IDプロバイダ**の**設定**リンクをクリックします。

    [![Screenshot](/images/2025-02-15_19-15-57.png)](/images/2025-02-15_19-15-57.png)

3. 以下の内容で設定し、**保存**ボタンをクリックします。

    | **Check Point Quantum Sparkの設定項目** | **設定内容** |
    | :--- | :--- |
    | **SAML ID プロバイダが必要なデータ** ||
    | **一意の識別子 URL** | [SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順8で使用します。 |
    | **返信 URL** | [SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順9で使用します。 |
    | **SAML ID プロバイダから受信したデータ（手動で挿入）** ||
    | **プロバイダ ID** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | **ログイン URL** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013/protocol/saml`） |
    | **証明書** | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDwYDVQQDDAg3MDAwMDA4MTA……..`）をコピーして、テキストエディタに貼り付けます。そして、先頭行に、`-----BEGIN CERTIFICATE-----`の文字列をコピーして、貼り付け、最終行に、`-----END CERTIFICATE-----`の文字列をコピーして貼り付け、拡張子を.cerとして保存します。<br>そして、そのファイルをアップロードします。 |

    [![Screenshot](/images/2025-02-15_19-11-12.png)](/images/2025-02-15_19-11-12.png)

## プロビジョニング
Check Point Quantum Sparkのプロビジョニングは不要です。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認方法
### リモートアクセスVPNの認証（SAML認証）

#### リモートアクセスクライアントのインストール
Check Point Quantum Sparkのリモートアクセスクライアントである、 Check Point Remote Access VPN Clients をインストールしていない場合には、Check Point Support Centerよりダウンロードしてインストールします。

[Check Point Support Center](https://support.checkpoint.com/){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. Check Point Remote Access VPN Clientsを起動して、新規サイトを作成します。サイトウィザードが表示されたら、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-24.png)](/images/image-24.png)

2. **Server address or Name**に、Check Point Quantum SparkのWANのIPアドレスを入力し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-25.png)](/images/image-25.png)

3. **SAML User (Default)**が選択されていることを確認し、**Next**ボタンをクリックします。

    [![Screenshot](/images/2025-02-15_19-48-58.png)](/images/2025-02-15_19-48-58.png)

4. **Site created successfully**が表示されたら、サイトが無事作成されました。**Finish**ボタンをクリックします。

    [![Screenshot](/images/image-29.png)](/images/image-29.png)

#### VPN接続

1. **Connect**ボタンをクリックします。 

    [![Screenshot](/images/image-30.png)](/images/image-30.png)

2. **Site**に、作成した接続先を選択します。

3. ログインを試み、ログインが成功することを確認します。

    [![Screenshot](/images/2025-02-15_19-53-32.png)](/images/2025-02-15_19-53-32.png)

4. **Details**ボタンをクリックすると、詳細が表示され、Identitiy Provider認証が行われて、接続が成功したことが確認できます。

    [![Screenshot](/images/2025-02-15_19-58-32.png)](/images/2025-02-15_19-58-32.png)

