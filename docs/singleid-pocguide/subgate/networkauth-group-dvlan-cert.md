# ネットワーク接続の認証（グループによるアクセス制限）とダイナミックVLANの併用-クライアント証明書認証
## 目的
SingleIDのユーザで、SubGateにより構成されたネットワークにアクセスします。
接続する際の認証方式は、クライアント証明書認証（EAP-TLS）です。接続端末にVLAN IDを動的に割り当てます。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@poc.singleid.jp |

!!! info
    メールアドレス: 受信可能なメールアドレスを指定してください。

### グループの情報
| **グループ名** | **メンバー** | **割当てるVLAN ID** |
| :--- | :--- | :-- |
| singleid-network-access-users | user1 | 101 |

### RADIUSの情報

| **設定に必要な情報** | **説明および情報取得の方法など** |
| :--- | :--- |
| **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
| **RADIUSサーバのIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**IPアドレス**です。 |
| **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**RADIUSポート番号**です。ここでは、デフォルトUDP1812を使用します。 |
| **RADIUSクライアントのIPアドレス** | **SubGate**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**Subgate-1234**とします。SubGateによる仕様により、アルファベット大文字・小文字、数字、記号を各1文字以上使用した9文字以上の文字列を使用します。|

### SubGateのポート
| **ポート番号** | **ポート名** | **802.1x認証<br>の有効化** | **トランク<br>ポート** | **アップリンク<br>ポート（ルータへ接続）** |
| :-- | :-- | :-- | :-- | :-- |
| 1 | ge1 | :material-check: |||
| 2 | ge2 | :material-check: |||
| 3 | ge3 | :material-check: |||
| 4 | ge4 | :material-check: |||
| 5 | ge5 | :material-check: |||
| 6 | ge6 | :material-check: |||
| 7 | ge7 ||||
| 8 | ge8 || :material-check: (VLAN ID:101)| :material-check: |

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
3. カタログから**SubGate**の**登録**ボタンをクリックします。**SubGate**画面がポップアップします。
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

5. **ネットワークアクセスの認証**タブへ移動します。
6. **許可グループ**の設定で**許可したいグループ**[（参照）](#グループの情報)をダブルクリックし、許可へ移動させます。
7. **許可**へ移動させたグループの同じ行に、**割当てるVLAN ID**[（参照）](#グループの情報)を入力します。
8. **登録**ボタンをクリックします。

### SubGateの設定
SubGateにCLIでログインして設定します。

!!! warning
    GUIでは、802.1x認証の設定を行うことはできません。

#### mac-floodingの無効化
mac-floodingが有効の状態では、802.1x認証を有効にできないため、mac-floodingを無効化します。
```
SG2412G(config)#no mds mac-flooding enable
```

#### 送信元のIPアドレスとデフォルトルートの設定
SingleIDのクラウドRADIUSと通信するために、デフォルトVLAN(vlan1.1)に送信元となるIPアドレス　および　デフォルトルートをSubGateに設定します。送信元となるIPアドレスおよびデフォルトルートは、環境により異なるため適切なIPアドレスを設定します。

``` title="送信元のIPアドレス設定"
SG2412G(config)interface vlan1.1
SG2412G(config-if)ip address <送信元のIPアドレス>/<送信元のIPアドレスのネットマスク>
```

``` title="デフォルートの設定"
SG2412G(config)ip route 0.0.0.0/0 <デフォルトゲートウェイのIPアドレス>
```

#### VLANの作成
**割当てるVLAN ID**[（参照）](#グループの情報)のVLANを作成します。
``` title="VLANの作成"
SG2412G(config)#vlan database
SG2412G(config-vlan)#vlan <VLAN ID> bridge 1
```

#### アップリンクポートをトランクポートの設定
**[SubGateのポート]**(#subgateのポート)に従い、ルータと接続するSubgateの物理ポート（アップリンクポート）に対して、トランクの設定をします。

``` title="トランクポートの設定"
SG2412G(config)#interface <アップリンクポートのインターフェース名>
SG2412G(config-if)#switchport mode trunk
SG2412G(config-if)##switchport trunk allowed vlan add <VLAN ID>
```

#### AAA認証の有効化およびRADIUSサーバの登録

| **設定項目** | **設定内容** |
| :--- | :--- |
| **host** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
| **auth-poty** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
| **Key** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |
| **source-interface** | [送信元のIPアドレスとデフォルトルートの設定](#送信元のipアドレスとデフォルトルートの設定)の手順で設定した**送信元のIPアドレス**を指定 |

```
SG2412G(config)#aaa system-aaa-ctrl
SG2412G(config)#radius-server host <1つめのRADIUSサーバのIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
SG2412G(config)#radius-server host <2つめのRADIUSサーバのIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
SG2412G(config)#ip radius source-interface <送信元のIPアドレス> 1023
```

#### 802.1x認証の有効化
**[SubGateのポート]**(#subgateのポート)に従い、802.1x認証の設定を行います。

```
SG2412G(config)#dot1x system-auth-ctrl
SG2412G(config)#interface range <802.1x認証の有効化する物理ポート名>
SG2412G(config-if-range)#dot1x port-control auto
SG2412G(config-if-range)#dot1x extension mac-auth-bypass
SG2412G(config-if-range)#dot1x extension dynamic-vlan
```

!!! warning
    `dot1x extension multi-user`と`dot1x extension dynamic-vlan`の両方を同一の物理ポートで同時に有効にできません。

#### 設定を保存
```
SG2412G(config)#write memory
```

#### サンプルコンフィグ
[ダウンロード](./networkauth-subgate-switch-sampleconfig-mab-dvlan.txt){ target=_blank .md-button .md-button--primary }

## 動作確認方法
### ネットワーク接続の認証（EAP-TLS方式のクライアント証明書認証）
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

#### ネットワークへ接続（Windows 11 の場合）
1. PCのネットワークインターフェースの802.1x認証を有効化する必要があります。Windowsの**サービス設定**画面から**Wired AutoConfig**サービスをダブルクリックします。

    [![Screenshot](/images/2022-09-06_15-40-20.png)](/images/2022-09-06_15-40-20.png)

2. PC起動時に、ネットワークインターフェースの802.1x認証を有効化するために、**Wired AutoConfig**サービスの**スタートアップの種類**を**自動**に変更します。そして、**開始**ボタンをクリックして、802.1x認証を有効化します。

    [![Screenshot](/images/2022-09-06_15-40-39.png)](/images/2022-09-06_15-40-39.png)

3. PCをSubGateの802.1x認証を有効にしたポート（[SubGateのポート](#SubGateのポート)の**802.1x認証の有効化**を参照）へ接続します。

4. ログイン要求がポップアップします。**サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-00-04.png)](/images/2022-09-06_17-00-04.png)

5. **認証設定**の**編集**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_18-15-32.png)](/images/2022-09-06_18-15-32.png)


6. 以下を設定し、**保存**ボタンをクリックします。
    
    | 設定項目 | 設定内容 |
    | :--- | :--- |
    | EAPメソッド | スマートカードまたはその他の証明書(EAP-TLS) |

    [![Screenshot](/images/2022-09-06_18-16-00.png)](/images/2022-09-06_18-16-00.png)

7. **サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-22-12.png)](/images/2022-09-06_17-22-12.png)

8. 認証で使用する**SingleIDのユーザ**[（参照）](#ユーザの情報)のクライアント証明書を選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_18-04-32.png)](/images/2022-09-06_18-04-32.png)

9. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのホスト名** |
    | **発行元** | R3 |

    [![Screenshot](/images/2022-09-06_15-57-08.png)](/images/2022-09-06_15-57-08.png)

10. 接続成功したことを確認します。

    [![Screenshot](/images/2022-09-06_17-27-56.png)](/images/2022-09-06_17-27-56.png)
