# 有線LANアクセス-クライアント証明書認証
文書更新日:2025-04-09

## 目的
* SingleIDのユーザで、SubGateへアクセスします。
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
1. **SingleID 管理者ポータル＞ユーザ**タブへ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**（例：デフォルト）および**配布形式**（例：一般）を選択します。
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**SubGate**の**登録**ボタンをクリックします。**SubGate**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **SubGate**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **ネットワークアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        * **証明書認証（EAP-TLS）のみ許可**を**有効**にすると、クライアント証明書認証以外の認証方式のアクセスを拒否します。
        * 認証したユーザおよびグループにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

7. **MACアドレス認証バイパス**タブへ移動します。
8. 802.1x認証をサポートしていないデバイスの**MACアドレス**を大文字英数字の**ハイフン区切り**で入力します。（例：00-E1-5C-68-16-04）

    !!! info
        MACアドレスにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

9. **登録**ボタンをクリックします。

### SubGateの設定
1. SubGateにCLIでログインして設定します。

    !!! info
        GUIでは、802.1x認証の設定を行うことはできません。

2. **mac-flooding**を**無効化**します。

    !!! info
        mac-floodingが有効の状態では、802.1x認証を有効にできません。

    ```
    SG2412G(config)#no mds mac-flooding enable
    ```

3. **送信元のIPアドレス**と**デフォルトルート**の設定を行います。SingleIDのクラウドRADIUSと通信するために、デフォルトVLAN(vlan1.1)に送信元となるIPアドレス　および　デフォルトルートを設定します。送信元となるIPアドレスおよびデフォルトルートは、環境により異なるため適切なIPアドレスを設定します。

    ``` title="送信元のIPアドレス設定"
    SG2412G(config)interface vlan1.1
    SG2412G(config-if)ip address <送信元のIPアドレス>/<送信元のIPアドレスのネットマスク>
    ```

    ``` title="デフォルートの設定"
    SG2412G(config)ip route 0.0.0.0/0 <デフォルトゲートウェイのIPアドレス>
    ```

4. （ダイナミックVLANを利用する場合）VLANを作成します。

    ``` title="VLANの作成"
    SG2412G(config)#vlan database
    SG2412G(config-vlan)#vlan <VLAN ID> bridge 1
    ```

5. （ダイナミックVLANを利用する場合）アップリンクポートをトランクポートに設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **アップリンクポートのインターフェース名** | アップリンクポートのインターフェース名です。（例: ge8） |

    ``` title="トランクポートの設定"
    SG2412G(config)#interface <アップリンクポートのインターフェース名>
    SG2412G(config-if)#switchport mode trunk
    SG2412G(config-if)##switchport trunk allowed vlan add <VLAN ID>
    ```

6. 以下の内容で、RADIUSサーバを登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバのプライマリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **RADIUSサーバのセカンダリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **RADIUSサーバのポート番号** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **RADIUSクライアントのシークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **送信元のIPアドレス** | [送信元のIPアドレスとデフォルトルートの設定](#送信元のipアドレスとデフォルトルートの設定)の手順で設定した**送信元のIPアドレス**です。 |

    ```
    SG2412G(config)#aaa system-aaa-ctrl
    SG2412G(config)#radius-server host <RADIUSサーバのプライマリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    SG2412G(config)#radius-server host <RADIUSサーバのセカンダリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    SG2412G(config)#ip radius source-interface <送信元のIPアドレス> 1023
    ```

7. **802.1x認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **802.1x認証の有効化する物理ポート名** | 802.1x認証の有効化する物理ポート名です。（例: ge1-6） |

    ``` title="ダイナミックVLANを利用する場合"
    SG2412G(config)#dot1x system-auth-ctrl
    SG2412G(config)#interface range <802.1x認証の有効化する物理ポート名>
    SG2412G(config-if-range)#dot1x port-control auto
    SG2412G(config-if-range)#dot1x extension multi-user
    SG2412G(config-if-range)#dot1x extension mac-auth-bypass
    ```

    ``` title="ダイナミックVLANを利用しない場合"
    SG2412G(config)#dot1x system-auth-ctrl
    SG2412G(config)#interface range <802.1x認証の有効化する物理ポート名>
    SG2412G(config-if-range)#dot1x port-control auto
    SG2412G(config-if-range)#dot1x extension mac-auth-bypass
    SG2412G(config-if-range)#dot1x extension dynamic-vlan
    ```

    !!! info
        `dot1x extension multi-user`と`dot1x extension dynamic-vlan`の両方を同一の物理ポートで同時に有効にできません。

8. 設定を保存します。
    
    ```
    SG2412G(config)#write memory
    ```

[SubGateのサンプルコンフィグ1 ダウンロード](./networkauth-subgate-switch-sampleconfig.txt){ target=_blank .md-button .md-button--primary }

[SubGateのサンプルコンフィグ2 ダウンロード](./networkauth-subgate-switch-sampleconfig-mab.txt){ target=_blank .md-button .md-button--primary }

[SubGateのサンプルコンフィグ3 ダウンロード](./networkauth-subgate-switch-sampleconfig-mab-dvlan.txt){ target=_blank .md-button .md-button--primary }

## 動作確認方法
Windows端末からEAP-TLS（クライアント証明書認証）でネットワークアクセスの認証が可能なことを確認します。

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

### 設定
1. PCのネットワークインターフェースの802.1x認証を有効化する必要があります。Windowsの**サービス設定**画面から**Wired AutoConfig**サービスをダブルクリックします。

    [![Screenshot](/images/2022-09-06_15-40-20.png)](/images/2022-09-06_15-40-20.png)

2. PC起動時に、ネットワークインターフェースの802.1x認証を有効化するために、**Wired AutoConfig**サービスの**スタートアップの種類**を**自動**に変更します。そして、**開始**ボタンをクリックして、802.1x認証を有効化します。

    [![Screenshot](/images/2022-09-06_15-40-39.png)](/images/2022-09-06_15-40-39.png)

3. PCをSubGateの802.1x認証を有効にしたポートへ接続します。

4. ログイン要求がポップアップします。**サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-00-04.png)](/images/2022-09-06_17-00-04.png)

5. **認証設定**の**編集**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_18-15-32.png)](/images/2022-09-06_18-15-32.png)


6. 以下を設定し、**保存**ボタンをクリックします。
    
    | 設定項目 | 設定内容 |
    | :--- | :--- |
    | EAPメソッド | スマートカードまたはその他の証明書(EAP-TLS) |

    [![Screenshot](/images/2022-09-06_18-16-00.png)](/images/2022-09-06_18-16-00.png)

### 接続
1. **サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-22-12.png)](/images/2022-09-06_17-22-12.png)

2. [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザのクライアント証明書を選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_18-04-32.png)](/images/2022-09-06_18-04-32.png)

3. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
    | **発行元** | R3 |

    [![Screenshot](/images/2022-09-06_15-57-08.png)](/images/2022-09-06_15-57-08.png)

4. 接続成功したことを確認します。

    [![Screenshot](/images/2022-09-06_17-27-56.png)](/images/2022-09-06_17-27-56.png)

### MACアドレス認証バイパス
1. **802.1x認証を無効にした**PCをSubGateの802.1x認証を有効にしたポートへ接続します。
2. １分間ほど待ち、ネットワークへ接続できたことを確認します。

    !!! info
        SubGateで`#show dot1x brief`コマンドを実行して、接続状況を確認できます。