# リモートアクセスVPN-パスワード認証
## 目的
* SingleIDのユーザで、Check Point Quantum SparkへVPNを使ってリモートアクセスします。
* 接続する際の認証方式は、パスワードです。
* SingleIDの拡張RADIUSサーバを利用します。
* ユーザ/グループによるアクセス制限をします。

## 設定方法
### SingleIDのグループの作成
1. **SingleID 管理者ポータル＞グループ**画面へ移動します。
2. **グループ追加**をクリックします。**グループ追加**画面がポップアップします。
3. **グループ名**を入力し、**登録**ボタンをクリックします。

### SingleIDのユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。**グループ**タブをクリックします。
4. メンバーとなるグループを選択し、**登録**ボタンをクリックします。

### SingleIDの拡張RADIUSサーバの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞基本設定**画面へ移動します。
2. **拡張RADIUSサーバ＞RADIUSポート番号**の**登録**ボタンをクリックします。**拡張RADIUSサーバの登録**画面がポップアップします。

    | **設定項目** | **設定内容** |
    | :-- | :-- |
    | **使用するプロトコル** | UDP |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

3. **登録**ボタンをクリックします。

### Check Point Quantum SparkのRADIUSサーバの設定
1. **Check Point Quantum Spark GUI＞デバイス＞管理者**画面へ移動します。
2. **RADIUSの設定**をクリックします。**RADIUSサーバの設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IPv4アドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **ポート** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **共有秘密キー** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順2の**シークレット**に設定した文字列です。 |

4. **適用**ボタンをクリックします。

### Check Point Quantum Sparkのリモートアクセスユーザの認証の設定
1. **Check Point Quantum Spark GUI＞VPN＞認証サーバ**画面へ移動します。
2. **RADIUS ユーザの権限**をクリックします。**RADIUS設定**画面がポップアップします。
3. **RADIUS認証をユーザ認識、リモートアクセス、ホットスポットに有効にする**を:fontawesome-regular-square-check:し、**適用**ボタンをクリックします。

### サイト識別する属性の確認方法
Check Point Quantum Sparkが送信するNAS-IP-Address属性およびNAS-Identifier属性の属性値を確認します。

1. [動作確認方法](#動作確認方法)に従って、認証を試みます。
2. 認証が失敗します。
3. **SingleID 管理者ポータル＞ログ＞RADIUS認証ログ**画面へ移動します。
4. 認証失敗したログのNAS-IPまたはNAS-IDの項目を確認します。NAS-IPが、NAS-IP-Address属性の属性値です。NAS-IDが、NAS-Identifier属性の属性値です。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Check Point Quantum Spark**の**登録**ボタンをクリックします。**Check Point Quantum Spark**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 拡張 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性を選択します。 |
    | **属性値** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性の属性値を設定します。 |        

5. **VPNアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
7. **登録**ボタンをクリックします。

## 動作確認方法
### リモートアクセスVPNの認証（パスワード認証）

#### リモートアクセスクライアントのインストール
Check Point Quantum Sparkのリモートアクセスクライアントである、 Check Point Remote Access VPN Clients をインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://supportcenter.checkpoint.com/supportcenter/portal?version=&amp;os=&amp;productTab=downloads&amp;product=175&amp;eventSubmit_doShowproductpage=){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. Check Point Remote Access VPN Clientsを起動して、新規サイトを作成します。サイトウィザードが表示されたら、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-24.png)](/images/image-24.png)

2. **Server address or Name**に、Check Point Quantum SparkのWANのIPアドレスを入力し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-25.png)](/images/image-25.png)

3. **VPN Client (Default)**が選択されていることを確認し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-26.png)](/images/image-26.png)

4. **Username and Password**を選択し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-27.png)](/images/image-27.png)

6. **Site created successfully**が表示されたら、サイトが無事作成されました。**Finish**ボタンをクリックします。

    [![Screenshot](/images/image-29.png)](/images/image-29.png)

#### VPN接続

1. **Connect**ボタンをクリックします。 

    [![Screenshot](/images/image-30.png)](/images/image-30.png)

2. **Site**に、作成した接続先を選択します。

3. ログインを試み、ログインが成功することを確認します。

    [![Screenshot](/images/image-31.png)](/images/image-31.png)

4. **Details**ボタンをクリックすると、詳細が表示され、RADIUS認証が行われて、接続が成功したことが確認できます。

    [![Screenshot](/images/image-32.png)](/images/image-32.png)

