# 無線LANアクセス グループごとに、接続可能な無線LANアクセスポイントをSSIDで制限-パスワード認証
## 目的
SingleIDのユーザで、Buffalo AirStation Proの無線LANアクセスポイントへアクセスします。
接続する際の認証方式は、パスワードです。

パスワード認証方式でサポートしている認証プロトコルは以下です。

* EAP-TTLS-PAP
* PEAP(MSCHAPv2)

本ガイドでは、SingleIDのRADIUS機能により、グループごとに接続可能な無線LANアクセスポイントをSSIDで制御します。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@poc.singleid.jp |

!!! info
    メールアドレス: 受信可能なメールアドレスを指定してください。

### グループの情報
| **グループ名** | **メンバー** | **接続可能な無線LANアクセスポイントのSSID** | **動作** |
| :--- | :--- | :--- | :--- |
| restricted-wlan-access-users | user1 | restricted-wlan-access | このSSIDの無線LANアクセスポイントのみにアクセス可能 |

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **Buffalo AirStation Pro**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**buffalo**とします。 |

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
3. **ユーザ登録**画面の**基本情報**を入力します。作成するユーザは、[ユーザの情報](#ユーザの情報)を参照します。 **グループ**タブをクリックします。
4. **参加するグループ**[（参照）](#グループの情報)を選択し、**登録**ボタンをクリックします。

#### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Buffalo AirStation Pro**の**登録**ボタンをクリックします。**Buffalo AirStation Pro**画面がポップアップします。
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

5. **無線アクセスの認証**タブへ移動します。
6. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、**許可**へ移動させます。
7. **許可**へ移動させたグループの同じ行に、接続を許可したい無線LANアクセスポイントのSSID[（参照）](#グループの情報)を入力します。
8. **登録**ボタンをクリックします。

### Buffalo AirStation Proの設定
1. **Buffalo AirStation Pro GUI**へアクセスします。
2. 管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。
3. **Buffalo AirStation Pro GUI＞詳細設定＞Wi-Fi設定＞SSID設定**画面へ移動します。
4. **新規追加**ボタンをクリックします。**SSID設定 - SSIDの編集**画面を表示します。
5. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Wi-Fi** | 有効 |
    | **SSID** | 任意のSSIDを入力します。 |
    | **使用デバイス** | 環境に応じて選択します。2.4GHz、5GHz |

    [![Screenshot](/images/2022-09-28_4-25-21.png)](/images/2022-09-28_4-25-21.png)

    | **設定項目** | **設定内容** |    
    | :--- | :--- |
    | **Wi-Fiの認証** | WPA2/WPA3 Enterprise |
    | **RADIUS** | 本画面で個別にRADIUSサーバの設定をする |

    [![Screenshot](/images/2022-09-28_4-27-41.png)](/images/2022-09-28_4-27-41.png)

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **プライマリーRADIUSサーバー** | |
    | **サーバー名** | IPアドレス [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **認証ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **Accounting** | **使用する**のチェックを外す |
    | **Shared Secret** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    | **セカンダリーRADIUSサーバー** | |
    | **サーバー名** | IPアドレス [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **認証ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **Accounting** | **使用する**のチェックを外す |
    | **Shared Secret** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    | **Calling-Station-Id** | **"-"（ハイフン区切り、大文字）**を選択 |
    | **Called-Station-Id** | **"-"（ハイフン区切り、大文字）:SSID**を選択 |

    [![Screenshot](/images/2022-09-28_4-38-50.png)](/images/2022-09-28_4-38-50.png)

6. **修正保存**ボタンをクリックします。
7. **設定**ボタンをクリックして、設定内容を機器へ適用します。

## 動作確認方法
### 無線LANアクセスの認証（EAP-TTLS-PAP方式のパスワード認証）
#### Windows 10/11 の場合

[グループ情報の**動作**を参照](#グループの情報)して、接続可能な無線LANアクセスポイントのSSIDにのみ接続が成功することを確認します。以下は、接続成功のパターンです。接続を許可していないSSIDには、接続できないことも確認してください。

1. Buffalo AirStation Proの無線LANアクセスポイントのSSIDを選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/image-34.png)](/images/image-34.png)

2. **SingleIDのユーザ**[参照](#ユーザの情報)の認証情報を入力し、**OK**ボタンをクリックします。

    [![Screenshot](/images/image-41.png)](/images/image-41.png)

3. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのホスト名** |
    | **発行元** | R3 |

    [![Screenshot](/images/image-48.png)](/images/image-48.png)

4. 接続成功したことを確認します。

    [![Screenshot](/images/image-39.png)](/images/image-39.png)