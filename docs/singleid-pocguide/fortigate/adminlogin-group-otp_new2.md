# 管理者ログイン-2要素認証（パスワード認証＋ワンタイムパスワード認証）
文書更新日:2025-03-09

## 目的
* SingleIDのユーザで、FortiGateへ管理者権限でログインします。
* 接続する際の認証方式は、2要素認証（パスワード認証＋ワンタイムパスワード認証）です。
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
3. カタログから**FortiGate NGFW**の**登録**ボタンをクリックします。**FortiGate NGFW**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | 有効 |
    | **サーバ** | 標準 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **FortiGate NGFW**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **管理アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        * **2要素認証（OTP）のみ許可**を**有効**にすると、2要素認証（OTP）以外の認証方式のアクセスを拒否します。

    !!! info
        必要に応じて権限を設定します。権限の詳細については、FortiGateの管理者ガイドをご確認ください。

        * prof_admin
        * super_admin
        * super_admin_readonly
        * radius_user_access1（ユーザ独自で作成したプロファイル）
        * radius_user_access2（ユーザ独自で作成したプロファイル）
        * radius_user_access3（ユーザ独自で作成したプロファイル）

7. **登録**ボタンをクリックします。

### FortiGateのRADIUSサーバの設定
1. **FortiGate 管理GUI＞ユーザ＆認証＞RADIUSサーバ**画面へ移動します。
2. **新規作成**ボタンをクリックします。**新規RADIUSサーバ設定**画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意の文字列を設定します。（例：SingleID_RADIUS） |
    | **認証方式** | **指定 PAP**を選択 |
    | **プライマリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **セカンダリサーバ** | |
    | **IP/名前** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |

    !!! info
        **接続をテスト**ボタンをクリックし、接続が成功しない場合には、以下をご確認ください。
        
        * 設定内容が間違いないこと
        * FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること

    !!! warning
        SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、**ユーザクレデンシャルをテスト**ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。

    [![Screenshot](/images/fortigate-10.png)](/images/fortigate-10.png)

4. **OK**ボタンをクリックします。

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

### FortiGateの管理者の設定
1. **FortiGate 管理GUI＞システム＞管理者**画面へ移動します。
2. **新規作成＞管理者**ボタンをクリックします。**新規管理者**画面が表示されます。
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | 任意の文字列を設定します。（例：SingleID） |
    | **タイプ** | **リモートサーバグループのすべてのユーザと一致**を選択します。 |
    | **管理者プロファイル** | **admin_no_access**を選択します。 |
    | **ユーザグループ** | [FortiGateのユーザグループの設定](#fortigateのユーザグループの設定)の手順で設定したユーザグループの名前を選択します。（例：SingleID） |

    [![Screenshot](/images/fortigate-15.png)](/images/fortigate-15.png)

4. **OK**ボタンをクリックします。
5. グループ（[FortiGateのユーザグループの設定](#fortigateのユーザグループの設定)の手順で設定したユーザグループの名前）のメンバーは、**admin_no_access**プロファイルにより管理者権限はありません。しかし、以下のコマンドを実行することで、RADIUSサーバで認証が成功した場合に、指定したプロファイル（例：super_admin_readonly）で上書きできます。

    ``` title="FortiGate CLI"
    # config system admin
        edit "SingleID"
        set accprofile-override enable
    end
    ```

    !!! info
        Fortinet社の以下の情報を参考にさせていただきました。<br>
        [Technical Tip: Remote admin login with Radius selecting admin access account profile](https://community.fortinet.com/t5/FortiAuthenticator/Technical-Tip-Remote-admin-login-with-Radius-selecting-admin/ta-p/192308?externalID=FD36127){ target=_blank }

## 動作確認方法
2要素認証（パスワード＋ワンタイムパスワード）による管理者ログインの認証が可能なことを確認します。

### ソフトウェアトークンのインストール
ソフトウェアトークンとして、以下のiPhoneおよびAndroidのモバイルアプリが利用できます。どちらかのアプリをスマートフォンまたはタブレットへインストールします。

* FreeOTP
* Google Authenticator

### SingleIDへソフトウェアトークンの登録

!!! warning
    **SingleIDへソフトウェアトークンの登録**を行っていないユーザは、ワンタイムパスワードは無効となり、パスワードでの認証となります。

1. [SingleIDのユーザの作成]の手順で作成したユーザで、SingleIDの[**ユーザポータル**](https://login.singleid.jp/)へログインします。
2. **SingleID ユーザポータル＞オーセンティケーター**画面へ移動します。
3. **QRコード**が表示されていることを確認します。

    [![Screenshot](/images/image-account-1.png)](/images/image-account-1.png)

4. スマートフォンまたはタブレットへインストールしたソフトウェアトークンアプリを起動します。（ここでは、**Google Authenticator**を利用します。）
5. **＋**をクリックし、新規アカウントを追加します。

    [![Screenshot](/images/image-token-1.png)](/images/image-token-1.png)

6. **QRコードをスキャン**をクリックし、ユーザポータルに表示されているQRコードを読み取ります。

    [![Screenshot](/images/image-token-2.png)](/images/image-token-2.png)

7. **アカウントを追加**をクリックし、アカウントを追加します。

    [![Screenshot](/images/image-token-3.png)](/images/image-token-3.png)

8. **表示されている数字**を**SingleID ユーザポータル＞オーセンティケーター**画面の**ワンタイムコード**へ入力し、**保存**ボタンをクリックし、オーセンティケーターを登録します。

    [![Screenshot](/images/image-token-4.png)](/images/image-token-4.png)

    !!! info
        ソフトウェアトークンの6桁の数字の表示は、30秒ごとに変わります。変わる前に、オーセンティケーターの登録を完了させる必要があります。登録する途中で、ソフトウェアトークンの数字が変わってしまった場合には、変わった数字を登録します。

### FortiGate 管理GUIへログイン
1. **FortiGate 管理GUI** https://FortiGateの管理IPアドレス/ へアクセスします。
2. 以下の情報でログインを試みます。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザ |
    | **パスワード** | **ユーザのパスワード**と**ソフトウェアトークンに表示されたワンタイムパスワード**を:（コロン）でつなげた文字列を入力します。（例：password:123456）|

    [![Screenshot](/images/fortigate-16.png)](/images/fortigate-16.png)

3. ログインが成功することを確認します。

    [![Screenshot](/images/fortigate-17.png)](/images/fortigate-17.png)

### 補足
以下の画面が表示されたときには、CLIから**set accprofile-override enable**がセットされていることを確認してください。

[![Screenshot](/images/fortigate-18.png)](/images/fortigate-18.png)

``` title="FortiGate CLI"
# config system admin
(admin) # edit "SingleID"
(SingleID) # show
config system admin
    edit "SingleID"
        set remote-auth enable
        set accprofile "admin_no_access"
        set vdom "root"
        set wildcard enable
        set remote-group "SingleID"
        set accprofile-override enable
    next
end
```
