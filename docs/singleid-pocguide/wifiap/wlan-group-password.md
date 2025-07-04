# 無線LANアクセス-パスワード認証
文書更新日:2025-04-09

## 目的
* SingleIDのユーザで、無線LANアクセスポイントへアクセスします。
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
3. カタログから**RADIUS対応無線LANアクセスポイント**の**登録**ボタンをクリックします。**RADIUS対応無線LANアクセスポイント**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **標準**を選択します。 |
    | **サーバ番号** | 適切なサーバ番号を選択します。選択するサーバの番号により、RADIUSサーバのポート番号が異なります。**SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞RADIUSポート番号**にサーバの番号と通信ポート番号の対応が記載されています。 |
    | **IP or ホスト名** | **無線LANアクセスポイント**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
    | **シークレット** | 任意の文字列を設定します。英大文字、英小文字、数字、記号を組み合わせて、最低でも14文字以上の複雑な文字列を設定することをお勧めします。 |

5. **無線アクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        認証したユーザおよびグループにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

7. **登録**ボタンをクリックします。

### 無線LANアクセスポイントの設定
* 以下の内容を無線LANアクセスポイントに設定します。

    | **設定項目** | **設定内容** |    
    | :--- | :--- |
    | **認証方式** | **WPA2/WPA3 Enterprise**を選択します。 |
    | **RADIUS IPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**標準RADIUSサーバ＞IPアドレス**の**プライマリ**および**セカンダリ**です。 |
    | **RADIUS 認証ポート** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **RADIUS シークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |

* **Called-Station-Id**属性に関する設定がある場合には、**無線LANアクセスポイントのMACアドレス:SSID**の形式になるように設定します。
* ユーザの再認証間隔に関する設定がある場合には、設定することをお勧めします。SingleIDのクラウドRADIUSは、**Session-Timeout**属性および**Termination-Action**属性をサポートしていないため、無線LANアクセスポイントのユーザの再認証間隔の設定に従いユーザの再認証が実施されます。再認証のタイミングで、通信が切れる無線LANアクセスポイントの場合には、短い再認証間隔は利便性に影響します。

## 動作確認方法
以下を参考にクライアントを設定して動作確認します。

[クライアント接続設定](../clients/index.md)
