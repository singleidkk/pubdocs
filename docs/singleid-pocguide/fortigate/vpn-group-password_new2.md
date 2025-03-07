# リモートアクセスVPN-パスワード認証
## 目的
* SingleIDのユーザで、FortiGateへVPNを使ってリモートアクセスします。
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
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**FortiGate NGFW**の**登録**ボタンをクリックします。**FortiGate NGFW**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 標準 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **FortiGate NGFW**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
    | **シークレット** | 任意の文字列を設定します。 |

5. **VPNアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
7. **登録**ボタンをクリックします。

### FortiGateのRADIUSサーバの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **プライマリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順4の**シークレット**に設定した文字列です。 |
    | **セカンダリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順4の**シークレット**に設定した文字列です。 |

    !!! info
        **接続をテスト**ボタンをクリックし、接続が成功しない場合には、以下をご確認ください。
        
        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

    [![Screenshot](/images/fortigate-1.png)](/images/fortigate-1.png)

4. **OK**ボタンをクリックします。

### FortiGateのユーザグループの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞ユーザグループ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規ユーザグループ**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID） |
    | **タイプ** | **ファイアウォール**を選択 |
    | **リモートグループ** | **リモートサーバ：SingleID_RADIUS（[FortiGateのRADIUSサーバの設定](#fortigateのradiusサーバの設定)の手順3で設定したサーバの名前）　グループ：すべて**を追加 |

    [![Screenshot](/images/fortigate-2.png)](/images/fortigate-2.png)

4. **OK**を クリックします。

### FortiGateのVPNの設定
1. **Fortigate 管理GUI＞VPN＞IPsecウィザード**画面へ移動します。
2. **新規作成＞管理者**ボタンをクリックします。
3. 以下の項目を設定し、**次へ**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：IPsecVPN） |
    | **テンプレートタイプ** | **リモートアクセス**を選択 |
    | **リモートデバイスタイプ** | **クライアントベース**　**FortiClient**を選択 |

    [![Screenshot](/images/fortigate-3.png)](/images/fortigate-3.png)

4. 以下の項目を設定し、**次へ**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **着信インターフェース** | FortiGateのWAN側のインターフェースです。環境により異なります。 |
    | **認証方式** | **事前共有鍵**を選択 |
    | **事前共有鍵** | 任意の文字列を設定します。ここでは、事前共有鍵を**fortigate**とします。 |
    | **ユーザグループ** | **SingleID**（[FortiGateのユーザグループの設定](#fortigateのユーザグループの設定)の手順3で設定したユーザグループの名前）を選択 |

    [![Screenshot](/images/fortigate-4.png)](/images/fortigate-4.png)

5. 以下の項目を設定し、**次へ**ボタンを クリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ローカルインターフェース** | FortiGateのLAN側のインターフェースです。環境により異なります。 |
    | **ローカルアドレス** | LAN側ネットワークを指定（例：internal） |
    | **クライアントアドレス範囲** | VPNクライアントに割り当てるIPアドレスの範囲です。環境により異なります。 |
    | **サブネットマスク** | 255.255.255.255 |

    [![Screenshot](/images/fortigate-5.png)](/images/fortigate-5.png)

6. **次へ**ボタンをクリックします。

    [![Screenshot](/images/fortigate-6.png)](/images/fortigate-6.png)

7. 設定を確認し、**作成**ボタンをクリックします。

## 動作確認方法
### リモートアクセスVPNの認証（パスワード認証）

#### リモートアクセスクライアントのインストール
FortiGateのリモートアクセスクライアントである、FortiClientをインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://www.fortinet.com/support/product-downloads#vpn){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. **FortiClient**を起動して、新規VPN接続を作成します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN** | **IPsec VPN**を選択 |
    | **接続名** | 任意の文字列を設定します。（例：SingleID） |
    | **リモートGW** | VPNクライアントが接続するIPアドレスです。環境により異なります。 |
    | **認証方法** | **事前共有鍵**を選択 |
    | **事前共有鍵** | [FortiGateのVPNの設定](#fortigateのvpnの設定)の手順4で設定した事前共有鍵 |
    | **認証（XAuth）** | **ユーザ名入力**を選択 |

    [![Screenshot](/images/fortigate-7.png)](/images/fortigate-7.png)

2. **保存**ボタンをクリックします。

#### VPN接続

1. **VPN名称**に、作成した接続先を選択します。
2. [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順6で許可したユーザでログインを試みます。

    [![Screenshot](/images/fortigate-8.png)](/images/fortigate-8.png)

3. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-9.png)](/images/fortigate-9.png)
