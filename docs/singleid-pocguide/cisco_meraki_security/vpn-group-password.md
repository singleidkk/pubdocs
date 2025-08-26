# リモートアクセスVPN(L2TP/IPSec)-パスワード認証
文書更新日:2025-08-24

## 目的
* SingleIDのユーザで、Cisco Meraki セキュリティ&SD-WANへVPN(L2TP/IPSec)を使ってリモートアクセスします。
* 接続する際の認証方式は、パスワードです。
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
Windows端末からパスワードによるリモートアクセスVPNの認証が可能なことを確認します。
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
7. **セキュリティ**タブへ移動します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **次のプロトコルを許可する** | 選択 |
    | **暗号化されていないパスワード（PAP）** | :fontawesome-regular-square-check: |
    | チャレンジハンドシェイク認証プロトコル（CHAP） | :fontawesome-regular-square: |
    | Microsoft CHAP Version2（MS-CHAP v2） | :fontawesome-regular-square: |
 
    [![Screenshot](/images/image-win-vpn-7.png)](/images/image-win-vpn-7.png)

    !!! info
        Cisco Meraki セキュリティ&SD-WANのクライアントVPN(L2TP over IPSec)は、PAPのみサポートしています。詳細は、[Cisco Merakiの技術情報/Client VPN Overview](https://documentation.meraki.com/MX/Client_VPN/Client_VPN_Overview){ target=_blank }を参照してください。

8. **OK**をクリックし、VPNの設定は完了です。

### VPNの接続
1. 設定したVPNの**接続**をクリックし、Cisco Meraki セキュリティ&SD-WANへVPN接続を試みます。

    [![Screenshot](/images/image-win-vpn-4.png)](/images/image-win-vpn-4.png)

2. 認証情報を入力し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |

    [![Screenshot](/images/image-win-vpn-5.png)](/images/image-win-vpn-5.png)

3. 接続成功したことを確認します。

    [![Screenshot](/images/image-win-vpn-6.png)](/images/image-win-vpn-6.png)
