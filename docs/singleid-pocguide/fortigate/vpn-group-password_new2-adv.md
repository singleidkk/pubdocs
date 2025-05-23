# リモートアクセスVPN-パスワード認証
文書更新日:2025-03-09

## 目的
* SingleIDのユーザで、FortiGateへVPNを使ってリモートアクセスします。
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
1. **SingleID 管理者ポータル＞認証＞RADIUS＞基本設定**タブへ移動します。
2. **拡張RADIUSサーバ＞RADIUSポート番号**の**登録**ボタンをクリックします。**拡張RADIUSサーバの登録**画面がポップアップします。

    | **設定項目** | **設定内容** |
    | :-- | :-- |
    | **使用するプロトコル** | **UDP**を選択します。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

3. **登録**ボタンをクリックします。専用のRADIUSポート番号が割り当てれます。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**FortiGate NGFW**の**登録**ボタンをクリックします。**FortiGate NGFW**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | **NAS-Identifier**を選択します。 |
    | **属性値** | 任意の文字列を設定します。（例：fortigate） |        

1. **VPNアクセスの認証**タブへ移動します。
2. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
3. **登録**ボタンをクリックします。

### FortiGateのRADIUSサーバの設定
1. **FortiGate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **プライマリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **シークレット** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |
    | **セカンダリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **シークレット** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |

    !!! info
        **接続をテスト**ボタンをクリックし、接続が成功しない場合には、以下をご確認ください。
        
        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

    [![Screenshot](/images/fortigate-1.png)](/images/fortigate-1.png)

4. **OK**ボタンをクリックします。
5. FortiGateのRADIUSのNAS-Identifierの属性値を変更するために、**FortiGate 管理GUI＞システム＞設定**画面へ移動します。以下のように設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ホスト名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サイトを識別する属性**の**属性値**に設定した文字列です。（例：fortigate） |

    !!! info
        FortiGateは、ホスト名をRADIUSのNAS-Identifier属性の属性値として、RADIUSリクエストを送信します。

6. FortiGateのRADIUSの認証通信ポートを変更するために、FortiGate CLIから以下のように設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバの名前** | 手順3で設定した名前です。（例：SingleID_RADIUS） |
    | **RADIUS認証の通信ポート番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順で割り当てられたポート番号です。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**です。 |

    ``` title="FortiGate CLI"
    config user radius
        edit "<RADIUSサーバの名前>"
            set radius-port <RADIUS認証の通信ポート番号>
        next
    end
    ```

### FortiGateのユーザグループの設定
1. **FortiGate 管理GUI＞ユーザ＆認証＞ユーザグループ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規ユーザグループ**画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID） |
    | **タイプ** | **ファイアウォール**を選択します。 |
    | **リモートグループ** | **リモートサーバ**：[FortiGateのRADIUSサーバの設定](#fortigateのradiusサーバの設定)の手順で設定したサーバの名前です。（例：SingleID_RADIUS）　**グループ**：**いずれか**を追加します。 |

    [![Screenshot](/images/fortigate-2.png)](/images/fortigate-2.png)

4. **OK**ボタンをクリックします。

### FortiGateのVPNの設定
1. **FortiGate 管理GUI＞VPN＞IPsecウィザード**画面へ移動します。
2. 以下の項目を設定し、**次へ**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：IPsecVPN） |
    | **テンプレートタイプ** | **リモートアクセス**を選択します。 |
    | **リモートデバイスタイプ** | **クライアントベース**　**FortiClient**を選択します。 |

    [![Screenshot](/images/fortigate-3.png)](/images/fortigate-3.png)

3. 以下の項目を設定し、**次へ**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **着信インターフェース** | FortiGateのWAN側のインターフェースです。環境により異なります。 |
    | **認証方式** | **事前共有鍵**を選択します。 |
    | **事前共有鍵** | 任意の文字列を設定します。 |
    | **ユーザグループ** | [FortiGateのユーザグループの設定](#fortigateのユーザグループの設定)の手順で設定したユーザグループの名前を選択します。（例：SingleID） |

    [![Screenshot](/images/fortigate-4.png)](/images/fortigate-4.png)

4. 以下の項目を設定し、**次へ**ボタンを クリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ローカルインターフェース** | FortiGateのLAN側のインターフェースです。環境により異なります。 |
    | **ローカルアドレス** | LAN側ネットワークを指定します。（例：internal） |
    | **クライアントアドレス範囲** | VPNクライアントに割り当てるIPアドレスの範囲です。環境により異なります。 |
    | **サブネットマスク** | **255.255.255.255**を設定します。 |

    [![Screenshot](/images/fortigate-5.png)](/images/fortigate-5.png)

5. **次へ**ボタンをクリックします。

    [![Screenshot](/images/fortigate-6.png)](/images/fortigate-6.png)

6. 設定を確認し、**作成**ボタンをクリックします。

## 動作確認方法
Windows端末からパスワードによるリモートアクセスVPNの認証が可能なことを確認します。

### リモートアクセスクライアントのインストール
FortiGateのリモートアクセスクライアントである、FortiClientをインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://www.fortinet.com/support/product-downloads#vpn){ target=_blank .md-button .md-button--primary }

### 接続先の設定

1. **FortiClient**を起動して、新規VPN接続を作成します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN** | **IPsecVPN**を選択します。 |
    | **接続名** | 任意の文字列を設定します。（例：SingleID） |
    | **リモートGW** | VPNクライアントが接続するIPアドレスです。 |
    | **認証方法** | **事前共有鍵**を選択します。 |
    | **事前共有鍵** | [FortiGateのVPNの設定](#fortigateのvpnの設定)の手順で設定した事前共有鍵です。 |
    | **認証（XAuth）** | **ユーザ名入力**を選択します。 |

    [![Screenshot](/images/fortigate-7.png)](/images/fortigate-7.png)

2. **保存**ボタンをクリックします。

### VPN接続

1. 以下を設定し、**接続**ボタンをクリックします。


    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN名称** | [接続先の設定](#vpn接続)の手順で作成した接続先を選択します。（例：SingleID） |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |

    [![Screenshot](/images/fortigate-8.png)](/images/fortigate-8.png)

2. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-9.png)](/images/fortigate-9.png)
