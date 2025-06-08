# 無線LANアクセス-パスワード認証
文書更新日:2025-06-08

## 目的
* SingleIDのユーザで、H3C 無線LANアクセスポイントへアクセスします。
* 接続する際の認証方式は、パスワード（PEAP、EAP-TTLS-PAP）です。
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
3. カタログから**H3C 無線LANアクセスポイント**の**登録**ボタンをクリックします。**H3C 無線LANアクセスポイント**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | **NAS-ID**を選択します。 |
    | **属性値** | 任意の文字列を設定します。 |

5. **無線アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        接続可能なSSIDを制限したい場合には、SSID入力欄に、接続を許可したいSSIDを入力します。

7. **登録**ボタンをクリックします。

### H3C 無線LANアクセスポイントの設定
#### RADIUS Schemeの登録
1. 機器のウェブ管理画面へログインします。
2. **Authentication＞AAA Settings＞RADIUS Settings**画面へ移動します。
3. **＋Add**ボタンをクリックし、新しい**RADIUS Scheme**を追加します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUS Scheme**| 任意の文字列を設定します。（例: singleid_radius） |
    | **Address** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **Port** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **Shared Key** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |

4. **Submit**ボタンをクリックします。
5. 追加した**RADIUS Scheme**の**編集**アイコンをクリックします。**Secondary Authentication Server**セクションの **＋Add**ボタンをクリックします。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Address** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **Port** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **Shared Key** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |
    | **Status** | **On**を選択します。|

6. **Submit**ボタンをクリックします。
7. **Advanced Configuration**セクションに以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Format for Usernames Sent to RADIUS Server** | **Based on User Input**を選択します。 |
    | **Server Response Timeout Timer** | デフォルト値の3秒では、インターネットの混雑次第ではタイムアウトする恐れがあります。タイムアウト値を長く設定することをお勧めします。(例: 10) |

8. **Submit**ボタンをクリックします。

#### ISP Domainの登録
機器のウェブ管理画面から設定します。

1. **Authentication＞AAA Settings＞ISP Domains**画面へ移動します。
2.  **＋Add**ボタンをクリックし、新しい**Domain**を追加します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ISP Domain Name** | 自社のドメイン名を設定します。（例: example.com） |
    | **ISP Domain State** | **Active**を選択します。 |
    | **User Access Methods** ||
    | **Authentication Scheme** | **Main Scheme**に**RADIUS Scheme**を選択します。**Scheme Name**に先ほど登録した**RADIUS Scheme**を選択します。（例: singleid_radius） |
    | **Authorization Scheme** | **Main Scheme**に**RADIUS Scheme**を選択します。**Scheme Name**に先ほど登録した**RADIUS Scheme**を選択します。（例: singleid_radius） |

3.  **Submit**ボタンをクリックします。

#### Wireless serviceの設定（WPA2エンタープライズの場合）
機器のウェブ管理画面から設定します。

1. **Wireless Services＞Wireless Service**画面へ移動します。
2.  **＋Add**ボタンをクリックし、新しい**Wireless service**を追加します。以下を設定します。

    * **Clone Service**画面

    **Skip**ボタンをクリックします。

    * **Create Service**画面

    以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Wireless service name** | 任意の文字列を設定します。（例: h3c） |
    | **SSID** | 任意の文字列を設定します。（例: h3c） |
    | **Status** | **On**を選択します。 |
    | **Forwarding Mode** | **Centralized Forwarding**を選択します。 |

    **Next**ボタンをクリックします。

    * **AuthN & Encryption**画面

    **Enterprise**を選択します。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Security Mechanism** | **802.1X**を選択します。 |
    | **Identity authentication** | **802.1X Auth(Encrypted)**を選択します。 |
    | **Encryption Mode** | **WPA2**を選択します。 |
    | **ISP Domain** | 先ほど登録した**ISP Domain**を選択します。（例: example.com） |

    **Next**ボタンをクリックします。

    * **Bind AP Group**画面

    **+Add**ボタンをクリックします。以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **AP Group Name** | APグループを選択します。（例: default-group） |
    | **Radio** | SSIDに関連付ける周波数帯を選択します。 |

    **Actions**の**OK**ボタンをクリックします。
    
3.  **Submit**ボタンをクリックします。

#### Wireless serviceの設定（WPA2/WPA3エンタープライズの場合）
機器のCLIから設定します。

1. **Wireless service**を登録します。

    ```
    system-view
    wlan service-template h3c <= 任意の文字列を設定します。（例: h3c）
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

1. APグループ（例: default-group）の**周波数帯域**に**Wireless service**を関連付けます。

    ```
    system-view
    wlan ap-group default-group
    radio 2.4
    service-template h3c-wpa3
    exit
    radio 5g
    service-template h3c-wpa3
    exit
    exit
    ```

3. 設定を保存します。

    ```
    save force
    ```

#### NAS-IDの設定
機器のウェブ管理画面から設定します。

1. **AP Config＞Global**画面へ移動します。
2. **Advanced Configuration**セクションの**NAS-ID**セクションに以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **NAS-ID** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順のNAS-IDの**属性値**に設定した文字列です。 |

3.  **Submit**ボタンをクリックします。

## 動作確認方法
Windows端末からパスワードで無線LANアクセスの認証が可能なことを確認します。

### 無線APへ接続
1. 無線LANアクセスポイントのSSIDを選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/image-34.png)](/images/image-34.png)

2. 認証情報を入力し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |

    [![Screenshot](/images/image-41.png)](/images/image-41.png)

3. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
    | **発行元** | R3 |

    [![Screenshot](/images/image-48.png)](/images/image-48.png)

4. 接続成功したことを確認します。

    [![Screenshot](/images/image-39.png)](/images/image-39.png)
