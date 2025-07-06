# 有線LANアクセス-クライアント証明書認証
文書更新日:2025-07-06

## 目的
* SingleIDのユーザで、Anti Spreader セキュアスイッチへアクセスします。
* 接続する際の認証方式は、クライアント証明書認証（EAP-TLS）です。
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

### SingleIDのクライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**タブへ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**（例：デフォルト）および**配布形式**（例：一般）を選択します。
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

### SingleIDの拡張RADIUSサーバの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞基本設定**タブへ移動します。
2. **拡張RADIUSサーバ＞RADIUSポート番号**の**登録**ボタンをクリックします。**拡張RADIUSサーバの登録**画面がポップアップします。

    | **設定項目** | **設定内容** |
    | :-- | :-- |
    | **使用するプロトコル** | **UDP**を選択します。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

3. **登録**ボタンをクリックします。専用のRADIUSポート番号が割り当てれます。

### Anti Spreader セキュアスイッチの設定
1. Anti Spreader セキュアスイッチにCLIでログインして設定します。

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
    | **RADIUSサーバのプライマリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **RADIUSサーバのセカンダリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **RADIUSクライアントのシークレット** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |
    | **送信元のIPアドレス** | [Anti Spreader セキュアスイッチの設定](#Anti Spreader セキュアスイッチの設定)の手順で設定した**送信元のIPアドレス**です。 |

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

[Anti Spreader セキュアスイッチのサンプルコンフィグ1 ダウンロード](./networkauth-antispreader-switch-sampleconfig.txt){ target=_blank .md-button .md-button--primary }

[Anti Spreader セキュアスイッチのサンプルコンフィグ2 ダウンロード](./networkauth-antispreader-switch-sampleconfig-mab.txt){ target=_blank .md-button .md-button--primary }

[Anti Spreader セキュアスイッチのサンプルコンフィグ3 ダウンロード](./networkauth-antispreader-switch-sampleconfig-mab-dvlan.txt){ target=_blank .md-button .md-button--primary }

### サイト識別する属性の確認方法
Anti Spreader セキュアスイッチが送信するNAS-IP-Address属性およびNAS-Identifier属性の属性値を確認します。

1. [動作確認方法](#動作確認方法)に従って、認証を試みます。
2. 認証が失敗します。
3. **SingleID 管理者ポータル＞ログ＞RADIUS認証ログ**画面へ移動します。
4. 認証失敗したログのNAS-IPまたはNAS-IDの項目を確認します。NAS-IPが、NAS-IP-Address属性の属性値です。NAS-IDが、NAS-Identifier属性の属性値です。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**Anti Spreader セキュアスイッチ**の**登録**ボタンをクリックします。**Anti Spreader セキュアスイッチ**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性を選択します。 |
    | **属性値** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性の属性値を設定します。<br>**サイト識別する属性**に**NAS-ID**を選択した場合、実際にネットワーク機器から送信される属性値に対して、ここの設定項目で設定した文字列で**部分一致検索**します。 |        

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

## 動作確認方法
以下を参考にクライアントを設定して動作確認します。

[クライアント接続設定](../clients/index.md)

### MACアドレス認証バイパス
1. **802.1x認証を無効にした**PCをAnti Spreader セキュアスイッチの802.1x認証を有効にしたポートへ接続します。
2. １分間ほど待ち、ネットワークへ接続できたことを確認します。

    !!! info
        Anti Spreader セキュアスイッチで`#show dot1x brief`コマンドを実行して、接続状況を確認できます。