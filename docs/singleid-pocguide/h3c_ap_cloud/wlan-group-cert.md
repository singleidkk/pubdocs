# 無線LANアクセス-クライアント証明書認証
文書更新日:2025-06-04

## 目的
* SingleIDのユーザで、H3C 無線LANアクセスポイントへアクセスします。
* 接続する際の認証方式は、クライアント証明書認証（EAP-TLS）です。
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

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**H3C 無線LANアクセスポイント**の**登録**ボタンをクリックします。**H3C 無線LANアクセスポイント**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **H3C 無線LANアクセスポイント**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。グローバルIPアドレスが、動的の場合には、DDNS（ダイナミックDNS）を利用して**ホスト名（FQDN）**を設定します。<br><br>**注意:** グローバルIPアドレスにIPv4の共用IPアドレスが使用されるインターネット接続サービス（IPv6インターネット接続、マンションタイプのインターネット接続、CATVインターネット、モバイルインターネット）の場合には、**拡張RADIUSサーバ**の利用を検討してください。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **無線アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        * **証明書認証（EAP-TLS）のみ許可**を**有効**にすると、クライアント証明書認証以外の認証方式のアクセスを拒否します。
        * 接続可能なSSIDを制限したい場合には、SSID入力欄に、接続を許可したいSSIDを入力します。

7. **登録**ボタンをクリックします。

### H3C 無線LANアクセスポイントの設定
#### RADIUS Schemeの登録
1. 機器のウェブ管理画面へログインします。
2. **Network Security＞AAA＞RADIUS**画面へ移動します。
3. **＋**ボタンをクリックし、新しい**RADIUS Scheme**を追加します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUS Scheme**| 任意の文字列を設定します。（例: singleid_radius） |

    **Authentication server**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Primary authentication server** ||
    | **VRF** | **Public network**を選択します。 |
    | **Type** | **IP address**を選択します。 |
    | **Host** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **Port** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **State** | **Active**を選択します。 |
    | **Secondary authentication server** ||
    | **VRF** | **Public network**を選択します。 |
    | **Type** | **IP address**を選択します。 |
    | **Host** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**標準RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **Port** | **Primary authentication server**の**Port**に設定したポート番号と同じです。 |
    | **State** | **Active**を選択します。 |
    |||
    | **The shared key for authentication** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |

    **Advanced Settings**

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **The RADIUS server response timeout time** | デフォルト値の3秒では、インターネットの混雑次第ではタイムアウトする恐れがあります。タイムアウト値を長く設定することをお勧めします。(例: 10) |

4. **Apply**ボタンをクリックします。

#### ISP Domainの登録
機器のウェブ管理画面から設定します。

1. **Network Security＞AAA＞ISP Domain**画面へ移動します。
2. **＋**ボタンをクリックし、新しい**Domain**を追加します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Domain** | 自社のドメイン名を設定します。（例: example.com） |
    | **State** | **Active**を選択します。 |
    | **Service type** | **LAN Access**を選択します。 |
    | **AAA for login users** ||
    | **Authentication** | **RADIUS**を選択します。先ほど登録した**RADIUS Scheme**を**Scheme**に選択します。（例: singleid_radius） |
    | **Authorization** | **RADIUS**を選択します。先ほど登録した**RADIUS Scheme**を**Scheme**に選択します。（例: singleid_radius） |

3. **Apply**ボタンをクリックします。

#### Wireless serviceの設定（WPA/WPA2エンタープライズの場合）
機器のウェブ管理画面から設定します。

1. **Wireless ConfigurationWireless＞Services Configuration**画面へ移動します。
2.  ＋ボタンをクリックし、新しい**Wireless service**を追加します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Basic settings** ||
    | **Wireless service name** | 任意の文字列を設定します。（例: 20） |
    | **SSID** | 任意の文字列を設定します。（例: h3c） |
    | **Wireless Service** | **On**を選択します。 |
    | **Authentication settings** ||
    | **Authentication mode** | **802.1X**を選択します。 |
    | **Security mode** | **WPA or WPA2**を選択します。 |
    | **Domain name** | 先ほど登録した**ISP Domain**を選択します。（例: example.com） |

3. **Apply and Configure Advanced settings**ボタンをクリックします。
4. **Binding**タブへ移動します。**Bind to APs**画面で、SSIDに関連付ける周波数帯を選択し、Selectedへ移動させます。
5. **Apply**ボタンをクリックします。
6. 画面右上の**SAVE**ボタンをクリックします。

#### Wireless serviceの設定（WPA2/WPA3エンタープライズの場合）
機器のCLIから設定します。

1. **Wireless service**を登録します。

    ```
    system-view
    wlan service-template 20 <= 任意の文字列を設定します。（例: 20）
    ssid h3c <= 任意の文字列を設定します。（例: h3c）
    akm mode dot1x
    cipher-suite ccmp
    security-ie rsn
    wpa3 enterprise-transition-mode <= WPA2/WPA3エンタープライズ（下位互換性を考慮し、推奨）
    client-security authentication-mode dot1x
    dot1x domain example.com <= 先ほど登録した**ISP Domain**を選択します。（例: example.com）
    bss transition-management enable
    service-template enable
    exit
    ```

2. **Wireless service**に**周波数帯域**を関連付けます。

    ```
    interface WLAN-Radio1/0/1 <= SSIDに関連付ける周波数帯のインターフェース
    service-template 20 <= 先ほど登録したWireless service
    exit
    interface WLAN-Radio1/0/2 <= SSIDに関連付ける周波数帯のインターフェース
    service-template 20 <= 先ほど登録したWireless service
    exit
    ```

3. 設定を保存します。

    ```
    save force
    ```

## 動作確認方法
Windows端末からEAP-TLS（クライアント証明書認証）で無線LANアクセスの認証が可能なことを確認します。

### クライアント証明書のダウンロード
1. **SingleIDシステム管理**から届いたメールを開きます。
2. **ダウンロード**リンクをクリックします。P12形式のクライアント証明書がダウンロードされます。
    [![Screenshot](/images/2021-09-02_19-10-04-1024x489-1.png)](/images/2021-09-02_19-10-04-1024x489-1.png)

!!! info
    もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードを スマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、 スマートフォンやタブレット にもクライアント証明書をダウンロードできます。

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

### 無線APへ接続
1. 無線LANアクセスポイントのSSIDを選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/image-34.png)](/images/image-34.png)

2. **証明書を使って接続する**をクリックします。

    [![Screenshot](/images/image-certwifilogin-1.png)](/images/image-certwifilogin-1.png)

3. 認証で使用するクライアント証明書を選択し、**OK**ボタンをクリックします。

    [![Screenshot](/images/image-certwifilogin-2.png)](/images/image-certwifilogin-2.png)

4. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
    | **発行元** | R3 |

    [![Screenshot](/images/image-48.png)](/images/image-48.png)

5. 接続成功したことを確認します。

    [![Screenshot](/images/image-39.png)](/images/image-39.png)
