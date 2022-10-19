# リモートアクセスVPN-２要素認証（パスワード認証＋クライアント証明書認証）
## 目的
SingleIDのユーザで、FortiGateへVPNを使ってリモートアクセスします。
接続する際の認証方式は、２要素認証（パスワード認証＋クライアント証明書認証）です。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@poc.singleid.jp |

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
1. SingleIDの[**管理者ポータル**](https://www.singleid.jp/product-login/){target=_blank}へログインします。

#### グループの作成
1. **SingleID 管理者ポータル＞グループ**画面へ移動します。
2. **グループ追加**をクリックします。**グループ追加**画面がポップアップします。
3. **グループ名**[（参照）](#グループの情報)を入力し、**登録**ボタンをクリックします。

#### ユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。作成するユーザは、[ユーザの情報](#ユーザの情報)を参照します。 

#### クライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。クライアント証明書を発行するユーザは、[ユーザの情報](#ユーザの情報)を参照します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**を選択します。（例：デフォルト）
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

#### SingleIDの中間CA証明書のダウンロード
1. **SingleID 管理者ポータル＞認証＞証明書**画面の**基本情報**タブへ移動します。
2. **中間CA**ブロックの中間CAのタイトル文字の隣の:fontawesome-solid-cloud-arrow-down:をクリックします。**ダウンロード**画面がポップアップします。
3. **ダウンロード**ボタンをクリックして、SingleIDの中間CA証明書をダウンロードします。

!!! warning
    誤って、ルートCA証明書をダウンロードしないようにご注意ください。ルートCA証明書のダウンロードは必要ありません。

#### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**FortiGate NGFW**の**登録**ボタンをクリックします。**FortiGate NGFW**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 1 |
    | **ワンタイムパスワード強制** | 無効 |
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

#### SingleIDの中間CA証明書を追加
1. **Fortigate 管理GUI＞システム＞証明書**画面へ移動します。
2. **Create/Import＞CA証明書**ボタンをクリックします。**CA証明書をインポート**画面がポップアップします。
3. 以下を設定し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **タイプ** | **ファイル**を選択 |
    | **アップロード** | [SingleIDの中間CA証明書のダウンロード](#singleidの中間ca証明書のダウンロード)の手順3でダウンロードしたファイルを選択 |
    
    !!! danger
        **SingleIDのルートCA証明書をFortiGateへインポートしないようにしてください。意図しないVPN接続の認証が成功してしまいます。**
        
        リモートアクセスクライアントからFortiGateへのVPN接続時、SingleIDのルートCA証明書がFortiGateにインポートされていると、SingleIDのルートCAを発行元とするどんな中間CAが発行したクライアント証明書であっても、証明書のパスの検証に成功してしまいます。

    [![Screenshot](/images/fortigate-19.png)](/images/fortigate-19.png)

#### SingleIDの中間CAの証明書失効リスト（CRL）の設定
1. **Fortigate 管理GUI＞システム＞証明書**画面へ移動します。
2. **Create/Import＞CRL**ボタンをクリックします。**CRLをインポート**画面がポップアップします。
3. 以下を設定し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **インポート方式** | **オンライン更新**を選択 |
    | **HTTPサーバのURL** | **SingleID 管理者ポータル＞認証＞証明書**画面の**基本情報**タブの**中間CAの失効リストの配布ポイント**です。 |

    [![Screenshot](/images/fortigate-20.png)](/images/fortigate-20.png)

4. 定期的（1時間ごと）に証明書失効リスト（CRL）を更新するために、FortiGate CLIから**set update-interval 3600**を以下のように設定します。

    ``` title="FortiGate CLI"
    config vpn certificate crl
        edit "CRL_1"
            set update-interval 3600
        next
    end
    ```

#### PKIユーザの登録
登録した中間CAで発行されたクライアント証明書かどうかを検証するために、以下のようにFortiGate CLIからPKIユーザを登録します。

``` title="FortiGate CLI"
config user peer
    edit "SingleID_Cert_Check"
        set ca "CA_Cert_1"
    next
end
```

#### RADIUSサーバの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **プライマリサーバ/セカンダリサーバ** | |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **IP/名前** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |

    !!! info
        **接続をテスト**ボタンをクリックし、接続が成功しない場合には、以下をご確認ください。
        
        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

    [![Screenshot](/images/fortigate-1.png)](/images/fortigate-1.png)

4. **OK**ボタンをクリックします。

#### ユーザグループの設定
1. **Fortigate 管理GUI＞ユーザ＆認証＞ユーザグループ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規ユーザグループ**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID_Cert） |
    | **タイプ** | **ファイアウォール**を選択 |
    | **リモートグループ** | **リモートサーバ：SingleID_RADIUS（[RADIUSサーバの設定](#radiusサーバの設定)の手順3で設定したサーバの名前）　グループ：すべて**を追加 |

    [![Screenshot](/images/fortigate-2.png)](/images/fortigate-2.png)

4. **OK**を クリックします。

#### SSLVPNの設定
1. **Fortigate 管理GUI＞VPN＞SSL-VPN設定**画面へ移動します。
2. 以下の項目を設定し、**適用**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **リッスンするインターフェース** | FortiGateのWAN側のインターフェースです。環境により異なります。 |
    | **リッスンするポート** | VPNクライアントが接続するポート番号です。ここでは、「10443」とします。 |
    | **サーバ証明書** | VPNのサーバ認証のためのサーバ証明書を指定します。ここでは、「Fortinet_Factory」とします。 |
    | **クライアント証明書を要求** | 有効 |
    | **認証ポータルマッピング** | グループ：SingleID_Cert（[ユーザグループの設定](#ユーザグループの設定)の手順3で設定したユーザグループ名） ポータル：full-access<br><br>すべてのその他のユーザ/グループ　ポータル：web-access |

    [![Screenshot](/images/fortigate-22.png)](/images/fortigate-22.png)

3. FortiFate CLIから以下を設定します。

    ``` title="FortiGate CLI"
    config vpn ssl settings
        config authentication-rule
            edit 1
                set client-cert enable
                set user-peer "SingleID_Cert_Check"
            next
        end
    end
    ```

#### ポリシーの設定
1. SSLVPNの設定を適用すると、上部に以下のようなメッセージが表示されます。リンクをクリックし、ファイアウォールポリシーの設定を行います。表示されない場合には、**Fortigate 管理GUI＞ポリシー＆オブジェクト＞ファイアウォールポリシー**へ移動します。

    [![Screenshot](/images/fortigate-12.png)](/images/fortigate-12.png)

2. 以下の項目を設定し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列です。（例：SSLVPN） |
    | **着信インターフェース** | SSL-VPN tunnel Interface (ssl.root) |
    | **発信インターフェース** | VPN接続が完了し、内部通信が行われるときの発信元のインターフェースです。 環境により異なります。 |
    | **発信元** | アドレス：SSLVPN_TUNNEL_ADDR1　ユーザグループ：SingleID_Cert（[ユーザグループの設定](#ユーザグループの設定)の手順3で設定したユーザグループ名） |
    | **宛先** | VPNでアクセスした通信に対して、許可したい宛先です。環境により異なります。 |
    | **サービス** | all |
    | **アクション** | 許可 |

    [![Screenshot](/images/fortigate-13.png)](/images/fortigate-13.png)

## 動作確認方法
### リモートアクセスVPNの認証（クライアント証明書認証）
#### クライアント証明書のダウンロード
1. **SingleIDシステム管理**から届いたメールを開きます。
2. **ダウンロード**リンクをクリックします。P12形式のクライアント証明書がダウンロードされます。
    [![Screenshot](/images/2021-09-02_19-10-04-1024x489-1.png)](/images/2021-09-02_19-10-04-1024x489-1.png)

!!! info
    もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードを スマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、 スマートフォンやタブレット にもクライアント証明書をダウンロードできます。


#### クライアント証明書のインストール
1. ダウンロードした**SingleIDのユーザ**[（参照）](#ユーザの情報)の P12形式のクライアント証明書ファイルをダブルクリックします。
2. 保存場所として**現在のユーザー**を選択し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-1.png)](/images/image-certinstall-1.png)

3. ダウンロードしたP12形式のファイル名とパスが正しく入力されていることを確認し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-2.png)](/images/image-certinstall-2.png)

4. **パスワード**に、クライアント証明書のパスワードを入力します。クライアント証明書のパスワードは、ユーザ名[（参照）](#ユーザの情報)と同一です。**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-3.png)](/images/image-certinstall-3.png)

5. **証明書の種類に基づいて、自動に証明書ストアを選択する**が選択されていることを確認し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-4.png)](/images/image-certinstall-4.png)

6. **完了**ボタンをクリックし、証明書をインストールします。

    [![Screenshot](/images/image-certinstall-5.png)](/images/image-certinstall-5.png)

7. クライアント証明書のインストールが成功しました。

    [![Screenshot](/images/image-certinstall-6.png)](/images/image-certinstall-6.png)

#### リモートアクセスクライアントのインストール
FortiGateのリモートアクセスクライアントである、FortiClientをインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://www.fortinet.com/support/product-downloads#vpn){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. **FortiClient**を起動して、新規VPN接続を作成します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN** | **SSL-VPN**を選択 |
    | **接続名** | 任意の文字列を設定します。（例：SingleID_Cert） |
    | **リモートGW** | VPNクライアントが接続するIPアドレスです。環境により異なります。 |
    | **ポートの編集** | [SSLVPNの設定](#sslvpnの設定)の手順2で設定した**リッスンするポート**です。 |
    | **クライアント証明書** | [クライアント証明書のインストール](#クライアント証明書のインストール)でインストールしたクライアント証明書を選択します。 |
    | **認証** | **ユーザ名入力**を選択 |

    [![Screenshot](/images/fortigate-26.png)](/images/fortigate-26.png)

2. **保存**ボタンをクリックします。

#### VPN接続

1. **VPN名称**に、作成した接続先を選択します。
2. **SingleIDのユーザ**[（参照）](#ユーザの情報)でログインを試みます。

    [![Screenshot](/images/fortigate-27.png)](/images/fortigate-27.png)

3. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-25.png)](/images/fortigate-25.png)