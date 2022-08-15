# 管理者ログイン-パスワード認証
## 目的
SingleIDのユーザで、Check Point FWへ管理者権限でログインします。
接続する際の認証方式は、パスワードです。

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
| singleid-system-administrators | user1 | 管理者ログイン可能 |

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **Check Point FW**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**checkpoint**とします。 |

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
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**CheckPoint**タブへ移動します。
2. **登録**ボタンをクリックします。**CheckPoint**画面がポップアップします。
3. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 1 |
    | **ワンタイムパスワード強制** | 無効 |
    | **IP or ホスト名** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのIPアドレス**を参照 |
    | **シークレット** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
    
    !!! info
        選択するサーバの番号により、RADIUSサーバのポート番号が異なります。サーバが1の場合には、UDP1812です。**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。

4. **管理アクセスの認証**タブへ移動します。
5. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、許可へ移動させます。
    
    !!! info
        必要に応じて権限を設定します。権限の詳細については、Check Point FWの管理者ガイドをご確認ください。

        * スーパー管理者
        * 読み取り専用管理者
        * ネットワーキング管理者
        * モバイル管理者

6. **登録**ボタンをクリックします。

### Check Point FWの設定
#### ローカル管理者でログイン
1. **Check Point FW GUI** https://Check PointのIP:4434/ へアクセスします。
2. ローカル管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。

#### RADIUSサーバの設定
1. **Check Point FW GUI＞デバイス＞管理者**画面へ移動します。
2. **RADIUSの設定**をクリックします。**RADIUSサーバの設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IPv4アドレス** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
    | **ポート** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
    | **共有秘密キー** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |

4. **適用**ボタンをクリックします。

#### 管理者ログインの認証の設定
1. **Check Point FW GUI＞デバイス＞管理者**画面へ移動します。
2. **権限の編集**をクリックします。**RADIUS認証**画面がポップアップします。
3. 以下を設定し、**適用**ボタンをクリックします。
    
    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **管理者のRADIUS認証を有効にする** | :fontawesome-regular-square-check: |
    | **RADIUSサーバで定義されたロールを使用** | 選択 |

## 動作確認方法
### 管理者ログインの認証（パスワード認証）

1. **Check Point FW GUI** https://Check PointのIP:4434/ へアクセスします。
2. **SingleIDのユーザ**[（参照）](#ユーザの情報)でログインを試み、ログインが成功することを確認します。

    [![Screenshot](/images/image-cp-login.png)](/images/image-cp-login.png)
