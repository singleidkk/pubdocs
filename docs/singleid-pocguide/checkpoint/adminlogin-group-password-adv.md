# 管理者ログイン-パスワード認証
## 目的
* SingleIDのユーザで、Check Point Quantum Sparkへ管理者権限でログインします。
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
1. **SingleID 管理者ポータル＞認証＞RADIUS＞基本設定**画面へ移動します。
2. **拡張RADIUSサーバ＞RADIUSポート番号**の**登録**ボタンをクリックします。**拡張RADIUSサーバの登録**画面がポップアップします。

    | **設定項目** | **設定内容** |
    | :-- | :-- |
    | **使用するプロトコル** | UDP |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

3. **登録**ボタンをクリックします。

### Check Point Quantum SparkのRADIUSサーバの設定
1. **Check Point Quantum Spark GUI＞デバイス＞管理者**画面へ移動します。
2. **RADIUSの設定**をクリックします。**RADIUSサーバの設定**画面がポップアップします。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **IPv4アドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **ポート** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**画面の**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **共有秘密キー** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順2の**シークレット**に設定した文字列です。 |

4. **適用**ボタンをクリックします。

### Check Point Quantum Sparkの管理者ログインの認証の設定
1. **Check Point Quantum Spark GUI＞デバイス＞管理者**画面へ移動します。
2. **権限の編集**をクリックします。**RADIUS認証**画面がポップアップします。
3. 以下を設定し、**適用**ボタンをクリックします。
    
    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **管理者のRADIUS認証を有効にする** | :fontawesome-regular-square-check: |
    | **RADIUSサーバで定義されたロールを使用** | 選択 |

### サイト識別する属性の確認方法
YAMAHA RTXが送信するNAS-IP-Address属性およびNAS-Identifier属性の属性値を確認します。

1. [動作確認方法](#動作確認方法)に従って、認証を試みます。
2. 認証が失敗します。
3. **SingleID 管理者ポータル＞ログ＞RADIUS認証ログ**画面へ移動します。
4. 認証失敗したログのNAS-IPまたはNAS-IDの項目を確認します。NAS-IPが、NAS-IP-Address属性の属性値です。NAS-IDが、NAS-Identifier属性の属性値です。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Check Point Quantum Spark**の**登録**ボタンをクリックします。**Check Point Quantum Spark**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 拡張 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性を選択します。 |
    | **属性値** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性の属性値を設定します。 |        

5. **管理アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。
    
    !!! info
        * 必要に応じて権限を設定します。権限の詳細については、Check Point Quantum Sparkの管理者ガイドをご確認ください。

            * スーパー管理者
            * 読み取り専用管理者
            * ネットワーキング管理者
            * モバイル管理者

7. **登録**ボタンをクリックします。

## 動作確認方法
### 管理者ログインの認証（パスワード認証）

1. **Check Point Quantum Spark GUI** https://Check PointのIP:4434/ へアクセスします。
2. ログインを試み、ログインが成功することを確認します。

    [![Screenshot](/images/image-cp-login.png)](/images/image-cp-login.png)

