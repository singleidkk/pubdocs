# リモートアクセスVPN(L2TP/IPSec)-2要素認証（パスワード認証＋ワンタイムパスワード認証）
文書更新日:2025-08-24

## 目的
* SingleIDのユーザで、Cisco Meraki セキュリティ&SD-WANへVPN(L2TP/IPSec)を使ってリモートアクセスします。
* 接続する際の認証方式は、2要素認証（パスワード認証＋ワンタイムパスワード認証）です。
* SingleIDの標準RADIUSサーバを利用します。
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

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Cisco Meraki セキュリティ&SD-WANシリーズ**の**登録**ボタンをクリックします。**Cisco Meraki セキュリティ&SD-WANシリーズ**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **Cisco Meraki セキュリティ&SD-WAN**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。グローバルIPアドレスが、動的の場合には、Cisco MerakiのDDNS（ダイナミックDNS）を利用して**ホスト名（FQDN）**を設定します。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **VPNアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        **2要素認証（OTP）のみ許可**を**有効**にすると、2要素認証（OTP）以外の認証方式のアクセスを拒否します。

7. **登録**ボタンをクリックします。

### Cisco Meraki セキュリティ&SD-WANのRADIUSサーバの設定
1. Cisco Merakiの管理画面へログインします。
2. **Security & SD-WAN＞Client VPN＞IPsec Settings**タブへ移動します。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Client VPN server** | **Enabled**を選択します。 |
    | **Hostname** | グローバルIPが動的の場合には、ここに表示されているホスト名を[SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**IP or ホスト名**に設定します。 |
    | **Subnet** | VPNクライアントに割り当てるIPアドレスのネットワークです。必要に応じて、ネットワークを作成してください。 |
    | **Shared secret** | IPSec事前共有鍵として任意の文字列を設定します。 |
    | **Authentication** | **RADIUS**を選択します。 |
    | **RADIUS servers** | **Add server**をクリックし、**プライマリ**および**セカンダリ**のRADIUSサーバを登録します。 |
    | **Host** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**および**セカンダリ**です。 |
    | **Port** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **Secret** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **RADIUS timeout** | **10秒**を設定します。 |
    | **Retry Count** | **3回**を設定します。 |

## 動作確認方法
Windows端末から2要素認証（パスワード＋ワンタイムパスワード）によるリモートアクセスVPNの認証が可能なことを確認します。

### ソフトウェアトークンのインストール
ソフトウェアトークンとして、以下のiPhoneおよびAndroidのモバイルアプリが利用できます。どちらかのアプリをスマートフォンまたはタブレットへインストールします。

* FreeOTP
* Google Authenticator

### SingleIDへソフトウェアトークンの登録

!!! warning
    **SingleIDへソフトウェアトークンの登録**を行っていないユーザは、ワンタイムパスワードは無効となり、パスワードでの認証となります。

1. [SingleIDのユーザの作成]の手順で作成したユーザで、SingleIDの[**ユーザポータル**](https://login.singleid.jp/)へログインします。
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

### VPNの設定
1. **Windowsの設定＞ネットワークとインターネット**画面へ移動します。
2. **VPN**をクリックします。
3. **VPN接続を追加する**をクリックします。

    [![Screenshot](/images/image-win-vpn-1.png)](/images/image-win-vpn-1.png)

4. 以下を設定します。**保存**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **接続名** | 任意の文字列を設定します。（例：SingleID） |
    | **サーバまたはアドレス** | VPNクライアントが接続するIPアドレスまたはホスト名(FQDN)です。 |
    | **VPNの種類** | **事前共有キーを使ったL2TP/IPSec**を選択します。 |
    | **事前共有キー** | [Cisco Meraki セキュリティ&SD-WANのRADIUSサーバの設定](#cisco-meraki-セキュリティsd-wanのradiusサーバの設定)の手順の**Shared secret**に設定した文字列です。 |
    | **サインイン情報の種類** | **ユーザ名とパスワード**を選択します。 |

    [![Screenshot](/images/image-win-vpn-2.png)](/images/image-win-vpn-2.png)

5. Windowsの**ネットワーク接続の設定**画面を開きます。
6. 追加したVPN設定の**プロパティ**を開きます。
7. **オプション**タブへ移動します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **資格情報を記憶する** | :fontawesome-regular-square: |

8. **セキュリティ**タブへ移動します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **次のプロトコルを許可する** | 選択 |
    | **暗号化されていないパスワード（PAP）** | :fontawesome-regular-square-check: |
    | チャレンジハンドシェイク認証プロトコル（CHAP） | :fontawesome-regular-square: |
    | Microsoft CHAP Version2（MS-CHAP v2） | :fontawesome-regular-square: |
 
    [![Screenshot](/images/image-win-vpn-7.png)](/images/image-win-vpn-7.png)

9. **OK**をクリックし、VPNの設定は完了です。

### VPNの接続
1. 設定したVPNの**接続**をクリックし、Cisco Meraki セキュリティ&SD-WANへVPN接続を試みます。

    [![Screenshot](/images/image-win-vpn-4.png)](/images/image-win-vpn-4.png)

2. 認証情報を入力し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**と**ソフトウェアトークンに表示されたワンタイムパスワード**を:（コロン）でつなげた文字列を入力します。（例：password:123456）|

    [![Screenshot](/images/image-win-vpn-5.png)](/images/image-win-vpn-5.png)

3. 接続成功したことを確認します。

    [![Screenshot](/images/image-win-vpn-6.png)](/images/image-win-vpn-6.png)
