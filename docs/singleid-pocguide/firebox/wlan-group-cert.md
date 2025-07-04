# 無線LANアクセス グループによるアクセス制限-パスワード認証
## 目的
SingleIDのユーザで、Fireboxの無線LANアクセスポイントへアクセスします。
接続する際の認証方式は、クライアント証明書認証（EAP-TLS）です。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@example.com |

!!! info
    メールアドレス: 受信可能なメールアドレスを指定してください。

### グループの情報
| **グループ名** | **メンバー** | **接続可能な無線LANアクセスポイントのSSID** | **動作** |
| :--- | :--- | :--- | :--- |
| singleid-wlan-access-users | user1 | SSIDの指定なし | すべての無線LANアクセスポイントへアクセス可能 |

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **Firebox**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**firebox**とします。 |

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

#### クライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。クライアント証明書を発行するユーザは、[ユーザの情報](#ユーザの情報)を参照します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**を選択します。（例：デフォルト）
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

#### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Firebox**の**登録**ボタンをクリックします。**Firebox**画面がポップアップします。
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
6. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、許可へ移動させます。

        !!! info
        接続可能なSSIDを制限したい場合には、SSID入力欄に、接続を許可したいSSIDを入力します。

7. **登録**ボタンをクリックします。

### Fireboxの設定
#### ローカル管理者でログイン
1. **Firebox 管理GUI** https://Fireboxの管理用IP:8080/ へアクセスします。
2. ローカル管理者のユーザ名、パスフレーズを入力し、**ログイン**をクリックします。

#### RADIUSサーバの設定
1. **Firebox 管理GUI＞認証＞サーバー**画面へ移動します。
2. **RADIUS**をクリックします。
3. **追加**ボタンをクリックします。**RADIUSサーバ**の設定画面が表示されます。
4. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **プライマリサーバーの設定/バックアップサーバーの設定** ||
    | **ドメイン名** | 任意（例：SingleID_RADIUS） |
    | **RADIUS サーバーの有効化** | 有効 |
    | **IP アドレス** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **共有シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    | **グループ属性** | 11（デフォルト値の11は、Filter-Id属性です。） |

5. **保存**ボタンをクリックして設定を保存します。

#### ワイヤレスの設定
1. **Firebox 管理GUI＞ネットワーク＞ワイヤレス**画面へ移動します。
2. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ワイヤレスを有効にする** | 有効 |
    | **ワイヤレス アクセス ポイントを有効にする** | 選択 |

3. **アクセス ポイント 1**の**構成**ボタンをクリックします。    

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **アクセス ポイントを有効にする 1** | 有効 |
    | **ネットワーク**タブ ||
    | **インターフェイス名 (エイリアス)** | 任意 |
    | **インターフェイスの種類** | 信頼済み |
    | **IP アドレス** | アクセスポイントのIPアドレスを設定します。 |
    | **DHCPサーバー** ||
    | **アドレスプール** | クライアントに割当てるIPアドレスの範囲です。 |
    | **ワイヤレス**タブ ||
    | **SSID をブロードキャストする** | 有効 |
    | **認証イベントの記録** | 有効 |
    | **ネットワーク名 (SSID)** | 任意 |
    | **暗号化（認証）** | WPA/WPA2エンタープライズ |
    | **暗号化アルゴリズム** | AES |
    | **認証サーバー** | AES |

4. **メインページに戻る**ボタンをクリックします。
5. **保存**ボタンをクリックします。

## 動作確認方法
### 無線LANアクセスの認証（EAP-TLS方式のクライアント証明書認証）
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

#### Fireboxの無線LANアクセスポイントへ接続（Windows 10/11 の場合）

1. Fireboxの無線LANアクセスポイントのSSIDを選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/image-34.png)](/images/image-34.png)

2. **証明書を使って接続する**をクリックします。

    [![Screenshot](/images/image-certwifilogin-1.png)](/images/image-certwifilogin-1.png)

3. 認証で使用する**SingleIDのユーザ**[（参照）](#ユーザの情報)のクライアント証明書を選択し、**OK**ボタンをクリックします。

    [![Screenshot](/images/image-certwifilogin-2.png)](/images/image-certwifilogin-2.png)

4. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのホスト名** |
    | **発行元** | R3 |

    [![Screenshot](/images/image-48.png)](/images/image-48.png)

5. 接続成功したことを確認します。

    [![Screenshot](/images/image-39.png)](/images/image-39.png)
