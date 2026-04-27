# リモートアクセスVPN-2要素認証（パスワード認証＋クライアント証明書認証）（IKEv2自動トランスポートモード）
文書更新日:2026-04-14

!!! info
    * 本設定ガイドは、FortiGate v7.6.6およびFortiClient 7.4.3.4726（ライセンス版）を用いて動作確認した内容に基づいて作成しています。
    * IKEv2の自動トランスポートモードでTCPへのフォールバックを利用するには、FortiClient 7.4.1以降が必要です。
    * Fortinet公式ドキュメントの「[FortiClient standalone and licensed version feature comparison](https://docs.fortinet.com/document/forticlient/7.4.3/administration-guide/269675/forticlient-standalone-and-licensed-version-feature-comparison)」によると、FortiClient 7.4系の無償Standalone版はIPsec over TCPをサポートしていません。UDPが利用できない環境でTCP接続まで確認する場合は、ライセンス版のFortiClientを使用してください。

## 目的
* SingleIDのユーザで、FortiGateへVPNを使ってリモートアクセスします。
* 接続する際の認証方式は、2要素認証（パスワード認証＋クライアント証明書認証）です。
* VPNトンネルは、**IKEv2の自動トランスポートモード**を利用します。
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

### SingleIDのクライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**（例：デフォルト）および**配布形式**（例：一般）を選択します。
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

### SingleIDのサーバ証明書の発行とダウンロード
1. **SingleID 管理者ポータル＞認証＞証明書＞証明書**タブへ移動します。
2. **発行**ボタンの▼をクリックし、プルダウンメニューの**サーバ証明書**を選択します。
3. **サーバ証明書の発行**画面がポップアップします。
4. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **サーバ名** | VPNクライアントが接続するIPアドレスまたはホスト名（FQDN）です。 |
    | **サブジェクトの代替名** | サーバ名にIPアドレスを設定した場合には、IPを選択します。サーバ名にホスト名を設定した場合には、DNSを選択します。 |
    | **有効期間** | 1年、5年、10年から選択します。（例：10年） |

5. **発行**ボタンをクリックして、サーバ証明書を発行します。
6. 発行されたサーバ証明書の行にある**チェックボックス**を選択します。
7. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書のダウンロード**を選択します。
8. ZIP圧縮されたP12形式のサーバ証明書がダウンロードされます。

    !!! info
        発行された証明書をデバイスへインストールする場合には、パスワードの入力が要求されます。パスワードは、サーバ名と同一です。（例：サーバ名が10.10.10.10の場合、サーバ証明書のパスワードは、10.10.10.10です。）

### SingleIDの中間CA証明書のダウンロード
1. **SingleID 管理者ポータル＞認証＞証明書＞基本情報**タブへ移動します。
2. **中間CA**ブロックの中間CAのタイトル文字の隣の:fontawesome-solid-cloud-arrow-down:をクリックします。**ダウンロード**画面がポップアップします。
3. **ダウンロード**ボタンをクリックして、SingleIDの中間CA証明書をダウンロードします。

!!! warning
    誤って、ルートCA証明書をダウンロードしないようにご注意ください。ルートCA証明書のダウンロードは必要ありません。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**FortiGate NGFW(IPsec IKEv2対応)**の**登録**ボタンをクリックします。**FortiGate NGFW(IPsec IKEv2対応)**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **FortiGate NGFW**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。グローバルIPアドレスが、動的の場合には、DDNS（ダイナミックDNS）を利用して**ホスト名（FQDN）**を設定します。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **VPNアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
7. **登録**ボタンをクリックします。

### FortiGateへ中間CA証明書を追加
1. **FortiGate 管理GUI＞システム＞証明書**画面へ移動します。

    !!! info
        メニューに**証明書**がない場合には、**FortiGate 管理GUI＞システム＞表示機能設定**画面で、**証明書**の表示を有効にします。

2. **作成/インポート＞CA証明書**ボタンをクリックします。**CA証明書をインポート**画面が表示されます。
3. 以下を設定し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **タイプ** | **ファイル**を選択します。 |
    | **アップロード** | [SingleIDの中間CA証明書のダウンロード](#singleidの中間ca証明書のダウンロード)の手順でダウンロードしたファイルを選択します。 |

    !!! danger
        **SingleIDのルートCA証明書をFortiGateへインポートしないようにしてください。意図しないVPN接続の認証が成功してしまいます。**

        リモートアクセスクライアントからFortiGateへのVPN接続時、SingleIDのルートCA証明書がFortiGateにインポートされていると、SingleIDのルートCAを発行元とするどんな中間CAが発行したクライアント証明書であっても、証明書のパスの検証に成功してしまいます。

    [![Screenshot](/images/fortigate-19.png)](/images/fortigate-19.png)

### FortiGateへ証明書失効リスト（CRL）の設定
1. **FortiGate 管理GUI＞システム＞証明書**画面へ移動します。
2. **作成/インポート＞CRL**ボタンをクリックします。**CRLをインポート**画面が表示されます。
3. 以下を設定し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **インポート方式** | **オンライン更新**を選択します。 |
    | **HTTPサーバのURL** | **SingleID 管理者ポータル＞認証＞証明書＞基本情報**タブの**中間CAの失効リストの配布ポイント**です。 |

    [![Screenshot](/images/fortigate-20.png)](/images/fortigate-20.png)

4. 定期的（1時間ごと）に証明書失効リスト（CRL）を更新するために、FortiGate CLIから**set update-interval 3600**を以下のように設定します。

    ``` title="FortiGate CLI"
    config vpn certificate crl
        edit "CRL_1"
            set update-interval 3600
        next
    end
    ```

### FortiGateへサーバ証明書を追加
1. **FortiGate 管理GUI＞システム＞証明書**画面へ移動します。
2. **作成/インポート＞証明書**ボタンをクリックします。**証明書の作成**画面が表示されます。
3. **証明書をインポート**ボタンをクリックします。
4. 以下を設定し、**作成**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **タイプ** | **PKCS12証明書**を選択します。 |
    | **キーファイルのある証明書** | [SingleIDのサーバ証明書の発行とダウンロード](#singleidのサーバ証明書の発行とダウンロード)の手順でダウンロードしたZIPファイルを解凍します。そして、拡張子がP12のファイルを選択します。 |
    | **パスワード** | 証明書ファイルの拡張子を除いた部分がパスワードです。（例：10.10.10.10.p12というファイルの場合には、パスワードは、10.10.10.10です。） |
    | **証明書名** | 証明書ファイルを選択しアップロードすると自動で入力されます。 |

    [![Screenshot](/images/2025-03-08_17-30-50.png)](/images/2025-03-08_17-30-50.png)

5. **OK**ボタンをクリックします。

### FortiGateのRADIUSサーバの設定
1. **FortiGate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **プライマリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **セカンダリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |

    [![Screenshot](/images/fortigate-1.png)](/images/fortigate-1.png)

4. **OK**ボタンをクリックします。
5. [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)で**サーバ番号2～10**を選択した場合は、FortiGate CLIからRADIUS認証の通信ポート番号を設定します。**サーバ番号1**を選択した場合は、FortiGateの初期値であるUDP 1812を使用するため、この手順は不要です。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバの名前** | 手順3で設定した名前です。（例：SingleID_RADIUS） |
    | **RADIUS認証の通信ポート番号** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)で選択したサーバ番号のポート番号です。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**を確認してください。 |

    ``` title="FortiGate CLI"
    config user radius
        edit "<RADIUSサーバの名前>"
            set radius-port <RADIUS認証の通信ポート番号>
        next
    end
    ```

    !!! info
        FortiGateのRADIUSサーバ設定画面で**接続をテスト**ボタンをクリックします。実行するタイミングは、選択したサーバ番号により異なります。

        * **サーバ番号1**を選択した場合：RADIUSサーバを作成した後
        * **サーバ番号2～10**を選択した場合：RADIUS認証の通信ポート番号を設定した後

        接続が成功しない場合には、以下をご確認ください。

        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスおよびRADIUS認証の通信ポート番号へ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

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
!!! info
    IKEv2の自動トランスポートモードの設定は、Fortinet公式ドキュメントの「[LDAP authentication with IKEv2 using UDP or TCP as transport](https://docs.fortinet.com/document/fortigate/7.6.6/administration-guide/442351/ldap-authentication-with-ikev2-using-udp-or-tcp-as-transport)」、「[Dialup IPsec VPN using custom TCP port](https://docs.fortinet.com/document/fortigate/7.6.6/administration-guide/567401/dialup-ipsec-vpn-using-custom-tcp-port)」、および「[IPsec IKEv2 VPN 2FA with EAP and certificate authentication](https://docs.fortinet.com/document/fortigate/7.6.6/administration-guide/298520/ipsec-ikev2-vpn-2fa-with-eap-and-certificate-authentication)」を参考にしています。

1. **FortiGate 管理GUI＞VPN＞VPNウィザード**画面へ移動します。
2. 以下の項目を設定し、**開始**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：IPsecVPN） |
    | **テンプレートを選択** | **リモートアクセス**を選択します。 |

3. 以下の項目を設定し、**次**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN クライアントの種類** | **FortiClient**を選択します。 |
    | **認証方式** | **シグネチャ**を選択します。 |
    | **証明書名** | [FortiGateへサーバ証明書を追加](#fortigateへサーバ証明書を追加)で追加したサーバ証明書を選択します。 |
    | **Peer certificate CA** | [FortiGateへ中間CA証明書を追加](#fortigateへ中間ca証明書を追加)の手順で追加した中間CAの証明書を選択します。 |
    | **IKE** | **バージョン2**を選択します。 |
    | **トランスポート** | **自動**を選択します。 |
    | **Fortinet カプセル化を使用する** | **無効**を選択します。 |
    | **NAT トラバーサル** | **有効**を選択します。 |
    | **EAP ピアの識別** | **EAP ID要求**を選択します。 |
    | **ユーザー認証方法** | **フェーズ1インターフェース**を選択します。[FortiGateのユーザグループの設定](#fortigateのユーザグループの設定)の手順で設定したユーザグループの名前を選択します。（例：SingleID） |

    [![Screenshot](/images/2026-04-14_10-54-24.png)](/images/2026-04-14_10-54-24.png)

4. 以下の項目を設定し、**次**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **接続されたエンドポイントに割り当てるアドレス** | VPNクライアントに割り当てるIPアドレスの範囲です。環境により異なります。（例：192.168.100.200-192.168.100.210） |
    | **接続されたエンドポイントのサブネット** | **255.255.255.255**を設定します。 |

    [![Screenshot](/images/2026-04-13_19-34-47.png)](/images/2026-04-13_19-34-47.png)

5. 以下の項目を設定し、**次**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **トンネルにバインドする着信インターフェイス** | FortiGateのWAN側のインターフェースです。環境により異なります。（例：wan1） |
    | **ローカルインターフェース** | FortiGateのLAN側のインターフェースです。環境により異なります。（例：internal） |
    | **ローカルアドレス** | LAN側ネットワークを指定します。（例：all） |

    [![Screenshot](/images/2026-04-13_19-42-11.png)](/images/2026-04-13_19-42-11.png)

6. 設定を確認し、**サブミット**ボタンをクリックします。

    !!! info
        FortiGate v7.6.6のVPNウィザードで作成した設定例は、以下を参照してください。<br>[ウィザード作成後のphase1-interface設定サンプル](./vpn-group-password-cert-ikev2-phase1-sample.txt)<br>[ウィザード作成後のphase2-interface設定サンプル](./vpn-group-password-cert-ikev2-phase2-sample.txt)

7. IKEv2でクライアント証明書認証とEAP認証を併用するために、VPNウィザードで作成したフェーズ1インターフェースに対して、`eap-cert-auth` をFortiGate CLIから有効にします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPNトンネル名** | 手順1で設定した名前です。（例：IPsecVPN） |

    ``` title="FortiGate CLI"
    config vpn ipsec phase1-interface
        edit "<VPNトンネル名>"
            set eap-cert-auth enable
        next
    end
    ```

    !!! info
        FortiGate v7.6.6のVPNウィザードで作成した設定では、`set peer "<VPNトンネル名>_peer"` は自動設定されますが、`set eap-cert-auth disable` のため、`set eap-cert-auth enable` への変更が必要です。

8. 自動モードでは、通常はUDPを使用し、UDPが利用できない場合にTCPへフォールバックします。デフォルト設定のまま利用する場合は、UDP側が**500**、TCP側が**443**であることを確認します。

    ``` title="FortiGate CLI"
    show full-configuration system settings | grep ike-
        set ike-port 500
        set ike-tcp-port 443
    ```

9. 自動モードではFortiGateがIKE用にTCPポート443を待ち受けるため、GUI管理ポートもポート443を使用していると、IPsecトンネルにバインドされているインターフェースのGUIアクセスに影響が出る可能性があります。必要に応じて管理アクセスポートを変更します。（例：10443）

    ``` title="FortiGate CLI"
    config system global
        set admin-sport 10443
    end
    ```

    !!! warning
        管理GUIへのアクセスが拒否されるため、管理GUIにアクセスするには、`https://<FortiGateの管理IPアドレス>:10443/` でログインしなおします。

    !!! info
        TCPポート443は、GUI管理アクセスのほか、ZTNAやAgentless VPNなど他の機能と競合する場合があります。FortiGateで利用している受信ポートとの競合がないか事前に確認してください。

## 動作確認方法
Windows端末から2要素認証（クライアント証明書＋パスワード）によるリモートアクセスVPNの認証が可能なことを確認します。

### クライアント証明書のダウンロード
1. **SingleIDシステム管理**から届いたメールを開きます。
2. **ダウンロード**リンクをクリックします。P12形式のクライアント証明書がダウンロードされます。

    [![Screenshot](/images/2021-09-02_19-10-04-1024x489-1.png)](/images/2021-09-02_19-10-04-1024x489-1.png)

!!! info
    もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードをスマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、スマートフォンやタブレットにもクライアント証明書をダウンロードできます。

### クライアント証明書のインストール
1. ダウンロードしたP12形式のクライアント証明書ファイルをダブルクリックします。
2. 保存場所として**現在のユーザー**を選択し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-1.png)](/images/image-certinstall-1.png)

3. ダウンロードしたP12形式のファイル名とパスが正しく入力されていることを確認し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-2.png)](/images/image-certinstall-2.png)

4. **パスワード**に、クライアント証明書のパスワードを入力します。クライアント証明書のパスワードは、ユーザ名と同一です。（例：ユーザ名が、user1の場合、クライアント証明書のパスワードは、user1です。）**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-3.png)](/images/image-certinstall-3.png)

5. **証明書の種類に基づいて、自動に証明書ストアを選択する**が選択されていることを確認し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/image-certinstall-4.png)](/images/image-certinstall-4.png)

6. **完了**ボタンをクリックし、証明書をインストールします。

    [![Screenshot](/images/image-certinstall-5.png)](/images/image-certinstall-5.png)

7. クライアント証明書のインストールが成功しました。

    [![Screenshot](/images/image-certinstall-6.png)](/images/image-certinstall-6.png)

### リモートアクセスクライアントのインストール
FortiGateのリモートアクセスクライアントである、FortiClientをインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://www.fortinet.com/support/product-downloads#vpn){ target=_blank .md-button .md-button--primary }

### 接続先の設定

1. **FortiClient**を起動して、新規VPN接続を作成します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN** | **IPsecVPN**を選択します。 |
    | **接続名** | 任意の文字列を設定します。（例：SingleID_Cert） |
    | **リモートGW** | VPNクライアントが接続するIPアドレスです。 |
    | **認証方法** | **X.509証明書**を選択します。 |
    | **クライアント証明書** | [クライアント証明書のインストール](#クライアント証明書のインストール)の手順でインストールしたクライアント証明書を選択します。 |
    | **認証（EAP）** | **ユーザ名入力**を選択します。 |

    [![Screenshot](/images/2026-04-14_12-41-12.png)](/images/2026-04-14_12-41-12.png)

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IKE** | **バージョン2**を選択します。 |
    | **Address Assignment** | **モードコンフィグ**を選択します。 |
    | **Encapsulation** | **Auto**を選択します。 |
    | **IKE UDP Port** | **500**を設定します。 |
    | **IKE TCP Port** | **443**を設定します。 |

    [![Screenshot](/images/2026-04-14_5-49-55.png)](/images/2026-04-14_5-49-55.png)

    フェーズ1およびフェーズ2の設定値は、FortiGate側のVPN設定に合わせてください。

    [![Screenshot](/images/2026-04-14_5-50-24.png)](/images/2026-04-14_5-50-24.png)
    [![Screenshot](/images/2026-04-14_5-50-49.png)](/images/2026-04-14_5-50-49.png)

2. **保存**ボタンをクリックします。

### VPN接続

1. 以下を設定し、**接続**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VPN名称** | [接続先の設定](#接続先の設定)の手順で作成した接続先を選択します。（例：SingleID_Cert） |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |
    | **クライアント証明書** | [クライアント証明書のインストール](#クライアント証明書のインストール)の手順でインストールしたクライアント証明書を選択します。 |

    [![Screenshot](/images/fortigate-27.png)](/images/fortigate-27.png)

2. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-25.png)](/images/fortigate-25.png)

3. FortiGate CLIで以下のコマンドを実行し、接続中のセッションが**UDP**か**TCP**かを確認します。

    ``` title="FortiGate CLI"
    diagnose vpn ike gateway list
    ```

    !!! info
        通常は`transport: UDP`となり、UDPが利用できないネットワーク環境では`transport: TCP`へ自動で切り替わります。
        サンプル出力は、[UDP接続時の出力例](./vpn-group-password-cert-ikev2-diagnose-udp-sample.txt) および [TCP接続時の出力例](./vpn-group-password-cert-ikev2-diagnose-tcp-sample.txt) を参照してください。
