# 無線LANアクセス-パスワード認証
文書更新日:2026-08-05

## 目的
* SingleIDのユーザで、Aruba Instant 無線LANアクセスポイントへアクセスします。
* 接続する際の認証方式は、パスワード（PEAP、EAP-TTLS-PAP）です。
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
3. カタログから**Aruba Instant 無線LANアクセスポイント**の**登録**ボタンをクリックします。**Aruba Instant 無線LANアクセスポイント**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **Aruba Instant 無線LANアクセスポイント**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。グローバルIPアドレスが、動的の場合には、DDNS（ダイナミックDNS）を利用して**ホスト名（FQDN）**を設定します。<br><br>**注意:** グローバルIPアドレスにIPv4の共用IPアドレスが使用されるインターネット接続サービス（IPv6インターネット接続、マンションタイプのインターネット接続、CATVインターネット、モバイルインターネット）の場合やDDNSサービスの利用ができない場合には、**拡張RADIUSサーバ**の利用を検討してください。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **無線アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        認証したユーザおよびグループにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

7. **登録**ボタンをクリックします。

### Aruba Instant 無線LANアクセスポイントの設定

#### RADIUSサーバの設定

1. Aruba Instantの管理画面へログインします。
2. **Configuration＞Security**画面へ移動します。
3. **Authentication Servers**を展開し、**＋**をクリックします。
4. プライマリRADIUSサーバを以下の内容で登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Name** | 任意の名前を設定します。（例: `SingleID-RADIUS-Primary`） |
    | **IP Address** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブに表示される標準RADIUSサーバのプライマリIPアドレスです。 |
    | **RadSec** | 無効にします。 |
    | **Auth port** | SingleIDのRADIUSサイトで選択したサーバ番号に対応するRADIUSポート番号です。（デフォルト値: 1812） |
    | **Shared key** | SingleIDのRADIUSサイトに設定したシークレットです。 |
    | **Retype key** | **Shared key**と同じ値です。 |
    | **Timeout** | **10秒**を設定します。（デフォルト値: 5秒） |

    [![Screenshot](/images/aruba-instant-radius-server-standard.png)](/images/aruba-instant-radius-server-standard.png)

5. **OK**をクリックします。
6. 同じ手順でセカンダリRADIUSサーバを登録します。（例: `SingleID-RADIUS-Secondary`）
7. **SAVE**ボタンをクリックします。

#### SSIDの設定

1. **Configuration＞Networks**画面へ移動します。
2. **＋**をクリックします。SSIDプロファイルを作成するウィザードが表示されます。
3. **Basic**ステップで、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Name** | 任意のSSIDを設定します。 |
    | **Type** | **Wireless**を選択します。 |
    | **Primary usage** | **Employee**を選択します。 |

    [![Screenshot](/images/aruba-instant-new-network-basic.png)](/images/aruba-instant-new-network-basic.png)

4. RADIUS属性でVLAN IDを割り当てる場合は、**VLAN**ステップで以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Client IP assignment** | **Network assigned**を選択します。 |
    | **Client VLAN assignment** | **Dynamic**を選択します。（デフォルト値: **Default**） |

    [![Screenshot](/images/aruba-instant-new-network-vlan.png)](/images/aruba-instant-new-network-vlan.png)

    !!! info
        **Client VLAN assignment**で**Default**を選択すると、有線ネットワークのネイティブVLANが割り当てられます。SingleIDで設定したVLAN IDは、RADIUSの`Tunnel-Type`、`Tunnel-Medium-Type`、`Tunnel-Private-Group-Id`により割り当てられるため、**Dynamic**を選択し、**VLAN Assignment Rules**は追加しません。

        **VLAN Assignment Rules**は、RADIUSから返された別の属性値を条件に、Aruba Instant側でVLANを割り当てる場合に使用します。

5. **Security**ステップで、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Security Level** | **Enterprise**を選択します。 |
    | **Key Management** | 検証対象に応じて**WPA2-Enterprise**または**WPA3-Enterprise (CCM 128)**を選択します。 |
    | **Authentication server 1** | 登録したプライマリRADIUSサーバを選択します。 |
    | **Authentication server 2** | 登録したセカンダリRADIUSサーバを選択します。 |
    | **EAP Offload** | デフォルトで無効です。無効であることを確認し、EAP認証をSingleIDのRADIUSサーバで処理します。 |
    | **Reauth interval** | `1440` **min.**を設定します。（デフォルト値: 0分） |
    | **Opportunistic Key Caching (OKC)** | 有効にします。（デフォルト値: 無効） |
    | **802.11r** | 必要に応じて設定します。 |
    | **802.11k** | 必要に応じて設定します。 |
    | **802.11k for 6G** | 6GHzを使用する場合に、必要に応じて設定します。 |
    | **802.11v** | 必要に応じて設定します。 |

    [![Screenshot](/images/aruba-instant-new-network-security.png)](/images/aruba-instant-new-network-security.png)

    !!! info
        **Reauth interval**を`1440`分に設定すると、接続中のクライアントを24時間ごとに再認証します。再認証に失敗したクライアントは切断されます。

        **Opportunistic Key Caching (OKC)**は、同一Instantクラスタ内のAP間を移動する際にPMKキャッシュを利用します。対応クライアントのローミング時に完全な802.1X認証を省略し、SingleIDへのRADIUS認証が高頻度に発生することを抑えるため、有効にします。

        **802.11r**、**802.11k**、**802.11k for 6G**、**802.11v**は、SingleIDとの連携に必要な設定ではありません。クライアントの対応状況と無線LANの運用方針に応じて設定してください。各機能の詳細は、[Configuring Security Settings for a WLAN SSID Profile（Aruba Instant）](https://arubanetworking.hpe.com/techdocs/Instant_8.x_WebHelp/Content/instant-ug/wlan-ssid-conf/conf-secur-sett.htm){ target=_blank }を確認してください。

6. **Access**ステップで、必要に応じてアクセスルールを設定し、**Finish**をクリックします。

    !!! info
        Aruba Instant側でアクセスを制限しない場合は、**Access Rules**に**Unrestricted**を選択し、**Download roles**を無効にします。

    [![Screenshot](/images/aruba-instant-new-network-access.png)](/images/aruba-instant-new-network-access.png)

7. Aruba InstantのCLIで、対象のSSIDプロファイルに以下を設定します。

    ```text
    configure terminal
    wlan ssid-profile <SSIDプロファイル名>
     called-station-id type macaddr
     called-station-id include-ssid delimiter :
    end
    commit apply
    ```

    !!! info
        この設定は、Called-Station-IdへSSIDを付加し、APのMACアドレスとSSIDの区切り文字にコロン（`:`）を使用するための設定です。

## 動作確認方法
以下を参考にクライアントを設定して動作確認します。

[クライアント接続設定](../clients/index.md)

## 参考情報

* [Aruba Instant 8.x Online Help](https://arubanetworking.hpe.com/techdocs/Instant_8.x_WebHelp/Content/homeinstant.htm){ target=_blank }
* [Aruba Instant CLI: wlan ssid-profile](https://arubanetworking.hpe.com/techdocs/CLI-Bank/Content/instant/wlan-ssid-prof.htm){ target=_blank }
* [Aruba Instant: Configuring Security Settings for a WLAN SSID Profile](https://arubanetworking.hpe.com/techdocs/Instant_8.x_WebHelp/Content/instant-ug/wlan-ssid-conf/conf-secur-sett.htm){ target=_blank }
