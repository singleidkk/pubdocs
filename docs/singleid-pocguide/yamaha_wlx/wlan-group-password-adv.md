# 無線LANアクセス-パスワード認証
文書更新日:2025-04-09

## 目的
* SingleIDのユーザで、YAMAHA WLXの無線LANアクセスポイントへアクセスします。
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

### YAMAHA WLXの設定
1. **YAMAHA WLX GUI＞無線設定＞SSID管理**画面へ移動します。
2. **追加**ボタンをクリックします。VAP設定画面を表示します。 
3. 以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **SSID**| 任意のSSIDを入力します。 |
    | **認証方式** | **WPA2-EAP**を選択します。 |
    | **暗号化方式** | **AES**を選択します。 |
    | **プライマリRADIUSサーバー** | **外部のRADIUSサーバーを使用する**を選択します。<br>IPアドレスは、**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **プライマリRADIUポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **プライマリRADIUSシークレット** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |
    | **セカンダリRADIUSサーバー** | **外部のRADIUSサーバーを使用する**を選択します。<br>IPアドレスは、**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **セカンダリRADIUポート番号** | **プライマリRADIUポート番号**に設定したポート番号と同じです。 |
    | **セカンダリRADIUSシークレット** | **プライマリRADIUSシークレット**に設定した文字列と同じです。 |

4. **設定**ボタンをクリックします。

### サイト識別する属性の確認方法
YAMAHA WLXが送信するNAS-IP-Address属性およびNAS-Identifier属性の属性値を確認します。

1. [動作確認方法](#動作確認方法)に従って、認証を試みます。
2. 認証が失敗します。
3. **SingleID 管理者ポータル＞ログ＞RADIUS認証ログ**画面へ移動します。
4. 認証失敗したログのNAS-IPまたはNAS-IDの項目を確認します。NAS-IPが、NAS-IP-Address属性の属性値です。NAS-IDが、NAS-Identifier属性の属性値です。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**YAMAHA WLXシリーズ**の**登録**ボタンをクリックします。**YAMAHA WLXシリーズ**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性を選択します。 |
    | **属性値** | [サイト識別する属性の確認方法](#サイト識別する属性の確認方法)で確認できた属性の属性値を設定します。<br>**サイト識別する属性**に**NAS-ID**を選択した場合、実際にネットワーク機器から送信される属性値に対して、ここの設定項目で設定した文字列で**部分一致検索**します。 |        

    !!! info
        YAMAHA WLXの場合には、NAS-IDは、IPアドレス、モデル名、機器特有の文字列で構成されています。
        例: 192.168.200.100/201-WLX413_Z0000000AL
        属性値は、部分一致により検索されるため、属性値を**WLX**と設定しておくと、DHCPで機器のIPアドレスが変更されてしまった場合や故障による機器交換があった場合でも、SingleIDのクラウドRADIUS側の設定変更が不要となります。

5. **無線アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        接続可能なSSIDを制限したい場合には、SSID入力欄に、接続を許可したいSSIDを入力します。

7. **登録**ボタンをクリックします。

## 動作確認方法
以下を参考にクライアントを設定して動作確認します。

[クライアント接続設定](../clients/index.md)
