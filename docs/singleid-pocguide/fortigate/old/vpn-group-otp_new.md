# リモートアクセスVPN-２要素認証（パスワード認証＋ワンタイムパスワード認証）
## 目的
SingleIDのユーザで、FortiGateへVPNを使ってリモートアクセスします。
接続する際の認証方式は、２要素認証（パスワード認証＋ワンタイムパスワード認証）です。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@example.com |

!!! info
    メールアドレス: 受信可能なメールアドレスを指定してください。

### グループの情報
| **グループ名** | **メンバー** | **動作** |
| :--- | :--- | :--- |
| singleid-remote-access-users | user1 | リモートアクセス可能 |

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **FortiGate**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**fortigate**とします。 |

## 設定方法
### SingleIDの設定
#### 管理者ポータルへログイン
1. SingleIDの[**管理者ポータル**](https://login.singleid.jp/){target=_blank}へログインします。

#### グループの作成
1. **SingleID 管理者ポータル＞グループ**画面へ移動します。
2. **グループ追加**をクリックします。**グループ追加**画面がポップアップします。
3. **グループ名**[（参照）](#グループの情報)を入力し、**登録**ボタンをクリックします。

#### ユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。作成するユーザは、[ユーザの情報](#ユーザの情報)を参照します。 **グループ**タブをクリックします。
4. **参加するグループ**[（参照）](#グループの情報)を選択し、**登録**ボタンをクリックします。

#### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**FortiGate NGFW**の**登録**ボタンをクリックします。**FortiGate NGFW**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 1 |
    | **ワンタイムパスワード強制** | 有効 |
    | **IP or ホスト名** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのIPアドレス**を参照 |
    | **シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    
    !!! info
        選択するサーバの番号により、RADIUSサーバのポート番号が異なります。サーバが1の場合には、UDP1812です。**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。

5. **VPNアクセスの認証**タブへ移動します。
6. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、許可へ移動させます。
7. **登録**ボタンをクリックします。

### FortiGateの設定
#### ローカル管理者でログイン
1. **Fortigate 管理GUI** https://FortiGateの管理IPアドレス/ へアクセスします。
2. ローカル管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。

#### RADIUSサーバの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **プライマリサーバ/セカンダリサーバ** | |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **認証方式** | **指定 PAP**を選択 |
    | **IP/名前** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |

    !!! info
        **接続をテスト**ボタンをクリックし、接続が成功しない場合には、以下をご確認ください。
        
        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

    [![Screenshot](/images/fortigate-10.png)](/images/fortigate-10.png)

4. **OK**ボタンをクリックします。

#### ユーザグループの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞ユーザグループ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規ユーザグループ**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID） |
    | **タイプ** | **ファイアウォール**を選択 |
    | **リモートグループ** | **リモートサーバ：SingleID_RADIUS（[RADIUSサーバの設定](#radiusサーバの設定)の手順3で設定したサーバの名前）　グループ：すべて**を追加 |

    [![Screenshot](/images/fortigate-2.png)](/images/fortigate-2.png)

4. **OK**を クリックします。

#### VPNの設定
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
    | **ユーザグループ** | **SingleID**（[ユーザグループの設定](#ユーザグループの設定)の手順3で設定したユーザグループの名前）を選択 |

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
### リモートアクセスVPNの認証（パスワード認証＋ワンタイムパスワード認証）

#### ソフトウェアトークンのインストール
ソフトウェアトークンとして、以下のiPhoneおよびAndroidのモバイルアプリが利用できます。どちらかのアプリをスマートフォンまたはタブレットへインストールします。

* FreeOTP
* Google Authenticator

#### SingleIDへソフトウェアトークンの登録

!!! warning
    **SingleIDへソフトウェアトークンの登録**を行っていないユーザは、ワンタイムパスワードは無効となり、パスワードでの認証となります。

1. **SingleIDのユーザ**[（参照）](#ユーザの情報)で、SingleIDの[**ユーザポータル**](https://login.singleid.jp/)へログインします。
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
    | **事前共有鍵** | [VPNの設定](#vpnの設定)の手順4で設定した事前共有鍵 |
    | **認証（XAuth）** | **ユーザ名入力**を選択 |

    [![Screenshot](/images/fortigate-7.png)](/images/fortigate-7.png)

2. **保存**ボタンをクリックします。

#### VPN接続

1. **VPN名称**に、作成した接続先を選択します。
2.  以下の情報でログインを試みます。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのユーザの情報](#ユーザの情報)を参照 |
    | **パスワード** | **ユーザのパスワード**と**ソフトウェアトークンに表示されたワンタイムパスワード**を:（コロン）でつなげた文字列を入力します。（例：password:123456）|

    [![Screenshot](/images/fortigate-8.png)](/images/fortigate-8.png)

3. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-9.png)](/images/fortigate-9.png)
