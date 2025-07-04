# リモートアクセスVPN-パスワード認証
## 目的
SingleIDのユーザで、Sophos FirewallへVPNを使ってリモートアクセスします。
接続する際の認証方式は、パスワードです。

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
| **RADIUSクライアントのIPアドレス** | **Sophos Firewall**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**sophos**とします。 |

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
3. カタログから**Sophos Firewall**の**登録**ボタンをクリックします。**Sophos Firewall**画面がポップアップします。
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

5. **その他の認証**タブへ移動します。
6. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、許可へ移動させます。
7. **グループ名**には、Sophos Firewallのグループ名を入力します。認証成功したユーザは、ここで指定したグループのメンバーとなります。指定しない場合には、**Open Group**のメンバーとなります。ここでは、**vpngroup**を設定します。
8. **登録**ボタンをクリックします。

### Sophos Firewallの設定
#### ローカル管理者でログイン
1. **Sophos Firewall 管理GUI** https://Sophosの管理用IP:4444/ へアクセスします。
2. ローカル管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。

#### RADIUSサーバの設定
1. **Sophos Firewall 管理GUI＞設定＞認証＞サーバー**画面へ移動します。
2. **追加**ボタンをクリックします。**認証サーバ**の設定画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **サーバの種類** | RADIUSサーバー |
    | **サーバ名** | 任意（例：SingleID_RADIUS） |
    | **サーバーIP** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **認証ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **共有シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    | **グループ名の属性** | Filter-Id |

4. **接続のテスト**ボタンをクリックし、[SingleIDのユーザ](#ユーザの情報)の認証情報を入力し、RADIUS認証が成功することを確認します。
5. 成功が確認出来たら**保存**ボタンをクリックして設定を保存します。

#### サービスの認証方法の設定
1. **Sophos Firewall 管理GUI＞設定＞認証＞サービス**画面へ移動します。
2. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザーポータルの認証方法** | [RADIUSサーバの設定](#radiusサーバの設定)で追加した認証サーバを選択します。ユーザポータルからユーザ自身でSophos Connect（VPNクライアントソフトウェア）をダウンロードするために、ユーザポータルへアクセス可能とするためです。 |
    | **SSL VPN 認証方法** | [RADIUSサーバの設定](#radiusサーバの設定)で追加した認証サーバを選択します。 |

#### グループの登録
1. **Sophos Firewall 管理GUI＞設定＞認証＞グループ**画面へ移動します。
2. **追加**ボタンをクリックし、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **グループ名** | vpngroup（[RADIUSの設定](#radiusの設定)の手順3で設定したグループ名です。） |
    | **グループの種類** | **ノーマル**を選択 |
    | **ネット閲覧クォータ** | **Unlimited Internet Access**を選択 |
    | **アクセス時間** | **Allowed all the time**を選択 |
    | **IPsec リモートアクセス** | **有効**を選択 |

3. **保存**ボタンをクリックします。

#### リモートアクセスVPN（SSL VPN）の設定
1. **Sophos Firewall 管理GUI＞設定＞リモートアクセスVPN＞SSL VPN**画面へ移動します。
2. **追加**ボタンをクリックします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意（例：SingleID_SSLVPN） |
    | **ポリシーメンバー** | vpngroup（[グループの登録](#グループの登録)で登録したグループ名です。） |
    | **デフォルトのゲートウェイとして使用** | VPN接続した際に、インターネット通信を含むすべての通信をSophos Firewallを経由させたい場合に、有効します。 |
    | **許可するネットワークリソース (IPv4)** | **デフォルトのゲートウェイとして使用**を無効にした場合には、接続可能なLAN側のネットワークを設定します。 |

4. **適用**ボタンをクリックします。

#### ファイアウォールルールの作成
1. **Sophos Firewall 管理GUI＞保護＞ルールとポリシー＞ファイアウォールルール**画面へ移動します。
2. **ファイアウォールルールの追加＞新しいファイアウォールルール**ボタンをクリックします。
3. VPNクライアントからアクセスしたいリソースへのルールを設定します。

## 動作確認方法
### Sophos Firewall ユーザポータルへログイン
1. **Sophos Firewall ユーザポータル** https://SophosのIP/ へアクセスします。
2. **SingleIDのユーザ**[（参照）](#ユーザの情報)でログインします。

### Sophos Connect（VPNクライアントソフトウェア）のダウンロードおよびインストール
1. **Sophos Firewall ユーザポータル＞VPN**画面へ移動します。
2. **Download for Windows**をクリックし、インストールファイル（SophosConnect_2.2.75_(IPsec_and_SSLVPN).msi）をダウンロードします。

    [![Screenshot](/images/2022-10-19_18-21-45.png)](/images/2022-10-19_18-21-45.png)

3. インストールファイル（SophosConnect_2.2.75_(IPsec_and_SSLVPN).msi）をダブルクリックして、インストールします。インストールには、Windowsの管理者権限が必要です。

### SSL VPN configurationのダウンロード
1. **Sophos Firewall ユーザポータル＞VPN**画面へ移動します。
2. **SSL VPN configuration**の**Windows、macOS、Linux 向けのダウンロード**をクリックし、SSL VPNの設定ファイル（ユーザ名__ssl_vpn_config.ovpn）をダウンロードします。

    [![Screenshot](/images/2022-10-19_18-23-51.png)](/images/2022-10-19_18-23-51.png)

### VPN接続の設定ファイルのインポート
1. Sophos Connectを起動します。
2. **接続のインポート**をクリックします。

    [![Screenshot](/images/2022-10-11_21-43-41.png)](/images/2022-10-11_21-43-41.png)

3. [SSL VPN configurationのダウンロード](#ssl-vpn-configurationのダウンロード)で取得したVPN接続の設定ファイルを選択します。

### リモートアクセスVPNの認証（パスワード認証）
1. **Sophos Connect**を起動します。
2. インポートした**VPN接続**を選択し、**接続**ボタンをクリックします。 

    [![Screenshot](/images/2022-10-19_18-28-09.png)](/images/2022-10-19_18-28-09.png)

3. [SingleIDのユーザ](#ユーザの情報)の認証情報を入力し、**サインイン**ボタンをクリックします。

    [![Screenshot](/images/2022-10-19_18-30-04.png)](/images/2022-10-19_18-30-04.png)

4. RADIUS認証が行われて、接続が成功したことが確認できます。

    [![Screenshot](/images/2022-10-19_18-31-18.png)](/images/2022-10-19_18-31-18.png)

