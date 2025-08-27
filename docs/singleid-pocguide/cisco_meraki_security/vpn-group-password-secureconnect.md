# リモートアクセスVPN(Cisco Secure Client)-パスワード認証
文書更新日:2025-08-24

## 目的
* SingleIDのユーザで、Cisco Meraki セキュリティ&SD-WANへVPN(Cisco Secure Client)を使ってリモートアクセスします。
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

5. **その他の認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
7. **登録**ボタンをクリックします。

### Cisco Meraki セキュリティ&SD-WANのRADIUSサーバの設定
1. Cisco Merakiの管理画面へログインします。
2. **Security & SD-WAN＞Client VPN＞Cisco Secure Client Settings**タブへ移動します。
3. 以下を設定します。

    * **Client VPN**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Cisco Secure Client VPN** | **Enabled**を選択します。 |

    * **Client Connection Details**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Hostname** | グローバルIPが動的の場合には、ここに表示されているホスト名を[SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**IP or ホスト名**に設定します。 |
    | **Cisco Secure Client Download Links** | VPNクライアントソフトをダウンロードします。 |
    | **Cisco Secure Client Port** | VPNクライアントが接続すれるポート番号です。(例: 443) |

    * **Secure connection between MX and Cisco Secure clients**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Server Certificate Generation Method** | **Auto-generated**を選択します。Cisco MerakiのDDNS（ダイナミックDNS）のホスト名に関連するサーバ証明書を利用します。<br>**注意**: SingleIDが発行するサーバ証明書は利用できません。 |

    * **Authentication and Access**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Authentication Type** | **RADIUS**を選択します。 |
    | **Certificate Authentication** | **Disabled**を選択します。 |
    | **RADIUS servers** | **Add server**をクリックし、**プライマリ**および**セカンダリ**のRADIUSサーバを登録します。 |
    | **Host** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**および**セカンダリ**です。 |
    | **Port** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **Secret** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **RADIUS timeout** | **10秒**を設定します。 |
    | **Cisco Secure Client VPN subnet** | VPNクライアントに割り当てるIPアドレスのネットワークです。必要に応じて、ネットワークを作成してください。 |

    !!! info
        **Group Policy with RADIUS Filter-Id**を**Enabled**にすることで、RADIUS認証成功時にSingleIDクラウドRADIUSが返すFilter-Id属性に基づき、FirewallルールなどのポリシーをVPNクライアントに適用することができます。<br>[SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順にある**その他の認証**タブ画面のグループ名のところに入力した文字列が、Filter-Id属性の値としてセットされます。

## 動作確認方法
Cisco Secure ClientからパスワードによるリモートアクセスVPNの認証が可能なことを確認します。

### VPNの接続
1. Cisco Secure Clientを起動します。
2. [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で設定した**IP or ホスト名**を入力し、**接続**ボタンをクリックし、Cisco Meraki セキュリティ&SD-WANへVPN接続を試みます。

    [![Screenshot](/images/2025-08-27_6-56-00.png)](/images/2025-08-27_6-56-00.png)

2. 認証情報を入力し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |

    [![Screenshot](/images/2025-08-27_7-09-23.png)](/images/2025-08-27_7-09-23.png)

3. 接続成功したことを確認し、**受け入れ**ボタンをクリックします。

    [![Screenshot](/images/2025-08-27_7-33-23.png)](/images/2025-08-27_7-33-23.png)
