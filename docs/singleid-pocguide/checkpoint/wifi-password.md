# WiFiアクセス-パスワード認証
## 目的
SingleIDのユーザで、Check Point FWのWiFIアクセスポイントへアクセスします。
接続する際の認証方式は、パスワードです。

## 環境
### ユーザの情報
user1

### グループの情報
wifi-access-users

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **Check Point FW**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**checkpoint**とします。 |

## 設定方法
### SingleIDの設定
#### 管理者ポータルへログイン
1. SingleIDの[**管理者ポータル**](https://www.singleid.jp/product-login/){target=_blank}へログインします。
2. **SingleID 管理者ポータル＞グループ**画面へ移動します。

#### グループの作成
1. **グループ追加**をクリックします。**グループ追加**画面がポップアップします。
2. **グループ名**を入力し、**登録**ボタンをクリックします。作成するグループは、[グループの情報](#グループの情報)を参照します。

#### ユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。作成するユーザは、[ユーザの情報](#ユーザの情報)を参照します。 **グループ**タブをクリックします。
4. 参加するグループを選択し、**登録**ボタンをクリックします。参加するグループについては、[グループの情報](#グループの情報)を参照します。

### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**CheckPoint**タブへ移動します。
2. **登録**ボタンをクリックします。**CheckPoint**画面がポップアップします。
3. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 1 |
    | **ワンタイムパスワード** | 無効 |
    | **IPアドレス** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのIPアドレス**を参照 |
    | **シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    
    !!! info
        選択するサーバの番号により、RADIUSサーバのポート番号が異なります。サーバが1の場合には、UDP1812です。**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。

4. **VPNアクセスの認証**タブへ移動します。
5. **許可グループ**の設定で許可したいグループをダブルクリックし、許可へ移動させます。許可するグループは、[グループの情報](#グループの情報)を参照します。
6. **登録**ボタンをクリックします。

### Check Point FWの設定
#### ローカル管理者でログイン
1. **Check Point FW GUI** https://Check PointのIP:4434/ へアクセスします。
2. ローカル管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。

#### RADIUSサーバの設定
1. **Check Point FW GUI＞デバイス＞管理者**画面へ移動します。
2. **RADIUSの設定**をクリックします。**RADIUSサーバの設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IPv4アドレス** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **共有秘密キー** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |

4. **適用**ボタンをクリックします。

#### リモートアクセスユーザの認証の設定
1. **Check Point FW GUI＞VPN＞認証サーバ**画面へ移動します。
2. **RADIUS ユーザの権限**をクリックします。**RADIUS設定**画面がポップアップします。
3. **RADIUS認証をユーザ認識、リモートアクセス、ホットスポットに有効にする**を✅し、**適用**ボタンをクリックします。

## 動作確認方法
### WiFiアクセスの認証（パスワード認証）

#### リモートアクセスクライアントのインストール
Check Point FWのリモートアクセスクライアントである、 Check Point Remote Access VPN Clients をインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://supportcenter.checkpoint.com/supportcenter/portal?version=&amp;os=&amp;productTab=downloads&amp;product=175&amp;eventSubmit_doShowproductpage=){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. Check Point Remote Access VPN Clientsを起動して、新規サイトを作成します。サイトウィザードが表示されたら、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-24.png)](/images/image-24.png)

2. **Server address or Name**に、Check Point FWのWANのIPアドレスを入力し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-25.png)](/images/image-25.png)

3. **VPN Client (Default)**が選択されていることを確認し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-26.png)](/images/image-26.png)

4. **Username and  Password**を選択し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-27.png)](/images/image-27.png)

6. **Site created successfully**が表示されたら、サイトが無事作成されました。**Finish**ボタンをクリックします。

    [![Screenshot](/images/image-29.png)](/images/image-29.png)

#### VPN接続

1. **Connect**ボタンをクリックします。 

    [![Screenshot](/images/image-30.png)](/images/image-30.png)

2. **Site**に、作成した接続先を選択します。

3. SingleIDのユーザ（user1）でログインを試み、ログインが成功することを確認します。

    [![Screenshot](/images/image-31.png)](/images/image-31.png)

4. **Details**ボタンをクリックすると、詳細が表示され、user1で、RADIUS認証が行われて、接続が成功したことが確認できます。

    [![Screenshot](/images/image-32.png)](/images/image-32.png)

