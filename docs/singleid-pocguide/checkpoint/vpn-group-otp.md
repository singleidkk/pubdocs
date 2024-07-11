# リモートアクセスVPN-2要素認証（パスワード認証＋ワンタイムパスワード認証）
## 目的
* SingleIDのユーザで、Check Point Quantum SparkへVPNを使ってリモートアクセスします。
* 接続する際の認証方式は、2要素認証（パスワード認証＋ワンタイムパスワード認証）です。
* SingleIDの標準RADIUSサーバを利用します。
* ユーザ/グループによるアクセス制限をします。

## 考慮事項
RADIUS連携時のCheckPointのVPN接続で、使用できるパスワードの文字数は**最大16文字**です。そのため、2要素認証（パスワード認証＋ワンタイムパスワード認証）をVPN接続で使用する場合には、**10文字**以上のパスワードが設定されたユーザではログインできません。

!!! info
    * ワンタイムパスワードで使用するトークンの長さ：6文字
    * パスワードとワンタイムパスワードトーンの区切り文字：1文字

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

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Check Point Quantum Spark**の**登録**ボタンをクリックします。**Check Point Quantum Spark**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 標準 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **Check Point Quantum Spark**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
    | **シークレット** | 任意の文字列を設定します。 |        

5. **VPNアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        * **２要素認証（OTP）のみ許可**を**有効**にすると、２要素認証（OTP）以外の認証方式のアクセスを拒否します。

7. **登録**ボタンをクリックします。

### Check Point Quantum SparkのRADIUSサーバの設定
1. **Check Point Quantum Spark GUI＞デバイス＞管理者**画面へ移動します。
2. **RADIUSの設定**をクリックします。**RADIUSサーバの設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IPv4アドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **ポート** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順4の**サーバ番号**に対応したポート番号です。 |
    | **共有秘密キー** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順4の**シークレット**に設定した文字列です。 |

4. **適用**ボタンをクリックします。

### Check Point Quantum Sparkのリモートアクセスユーザの認証の設定
1. **Check Point Quantum Spark GUI＞VPN＞認証サーバ**画面へ移動します。
2. **RADIUS ユーザの権限**をクリックします。**RADIUS設定**画面がポップアップします。
3. **RADIUS認証をユーザ認識、リモートアクセス、ホットスポットに有効にする**を:fontawesome-regular-square-check:し、**適用**ボタンをクリックします。

## 動作確認方法
### リモートアクセスVPNの認証（パスワード認証＋ワンタイムパスワード認証）

#### ソフトウェアトークンのインストール
ソフトウェアトークンとして、以下のiPhoneおよびAndroidのモバイルアプリが利用できます。どちらかのアプリをスマートフォンまたはタブレットへインストールします。

* FreeOTP
* Google Authenticator

#### SingleIDへソフトウェアトークンの登録

!!! warning
    **SingleIDへソフトウェアトークンの登録**を行っていないユーザは、ワンタイムパスワードは無効となり、パスワードでの認証となります。

1. SingleIDの[**ユーザポータル**](https://login.singleid.jp/)へログインします。
2. **SingleID ユーザポータル＞オーセンティケーター**画面へ移動します。
3. **QRコード**が表示されていることを確認します。

    [![Screenshot](/images/image-account-1.png)](/images/image-account-1.png)

4. スマートフォンまたはタブレットへインストールしたソフトウェアトークンアプリを起動します。（ここでは、**Google Authenticator**を利用します。）
5. **＋**をクリックし、新規アカウントを追加します。

    [![Screenshot](/images/image-token-1.png)](/images/image-token-1.png)

6. **QRコードをスキャン**をクリックし、ユーザポータルに表示されているQRコードを読み取ります。

    [![Screenshot](/images/image-token-2.png)](/images/image-token-2.png)

7. **アカウントを追加**をクリックし、アカウントを追加します。

    [![Screenshot](/images/image-token-3.png)](/images/image-token-3.png)

8. **表示されている数字**を**SingleID ユーザポータル＞オーセンティケーター**画面の**ワンタイムコード**へ入力し、**保存**ボタンをクリックし、オーセンティケーターを登録します。

    [![Screenshot](/images/image-token-4.png)](/images/image-token-4.png)

    !!! info
        ソフトウェアトークンの6桁の数字の表示は、30秒ごとに変わります。変わる前に、オーセンティケーターの登録を完了させる必要があります。登録する途中で、ソフトウェアトークンの数字が変わってしまった場合には、変わった数字を登録します。

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

3.  ログインを試み、ログインが成功することを確認します。なお、パスワードの形式は、**ユーザのパスワード**と**ソフトウェアトークンに表示されたワンタイムパスワード**を:（コロン）でつなげた文字列を入力します。（例：password:123456）

    [![Screenshot](/images/image-31.png)](/images/image-31.png)

4. **Details**ボタンをクリックすると、詳細が表示され、RADIUS認証が行われて、接続が成功したことが確認できます。

    [![Screenshot](/images/image-32.png)](/images/image-32.png)

