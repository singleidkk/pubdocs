# ネットワーク接続の認証（グループによるアクセス制限）とダイナミックVLANの併用-パスワード認証
## 目的
SingleIDのユーザで、YAMAHA SWXにより構成されたネットワークにアクセスします。
接続する際の認証方式は、パスワードです。認証成功した際に、接続端末にVLAN IDを動的に割り当てます。

パスワード認証方式でサポートしている認証プロトコルは以下です。

* EAP-TTLS-PAP
* PEAP(MSCHAPv2)

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@example.com |

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
| **RADIUSクライアントのIPアドレス** | **YAMAHA SWX**側の**グローバルIPアドレス**です。インターネットに出ていくときの送信元のIPアドレスです。 |
| **RADIUSクライアントのシークレット** | 任意の文字列を設定します。ここでは、シークレットを**yamaha**とします。|

### YAMAHA SWXのポート
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

#### RADIUSの設定
1. **SingleID 管理者ポータル＞認証＞RADIUS**画面の**簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**YAMAHA SWXシリーズ**の**登録**ボタンをクリックします。**YAMAHA SWXシリーズ**画面がポップアップします。
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

### YAMAHA SWXの設定
YAMAHA SWXにCLIでログインして設定します。

!!! info
    GUIによる設定も可能です。

#### デフォルトルートの設定
SingleIDのクラウドRADIUSと通信するために、デフォルトルートをYAMAHA SWXに設定します。デフォルトルートは、環境により異なるため適切なIPアドレスを設定します。

``` title="デフォルートの設定"
(config)#ip route 0.0.0.0/0 <デフォルトゲートウェイのIPアドレス>
```

#### VLANの作成
**割当てるVLAN ID**[（参照）](#グループの情報)のVLANを作成します。
``` title="VLANの作成"
(config)#vlan database
(config-vlan)#vlan <VLAN ID>
```

#### アップリンクポートをトランクポートの設定
[**YAMAHA SWXのポート**](#yamaha-swxのポート)に従い、ルータと接続するYAMAHA SWXの物理ポート（アップリンクポート）に対して、トランクの設定をします。

``` title="トランクポートの設定"
(config)#interface <アップリンクポートのインターフェース名>
(config-if)#switchport mode trunk
(config-if)#switchport trunk allowed vlan add <VLAN ID>
```

#### RADIUSサーバの登録

| **設定項目** | **設定内容** |
| :--- | :--- |
| **host** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのIPアドレス**を参照 |
| **auth-port** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのポート番号**を参照 |
| **Key** | [RADIUSの情報](#radiusの情報)の**RADIUSクライアントのシークレット**を参照 |

```
(config)#radius-server host <1つめのRADIUSサーバのIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
(config)#radius-server host <2つめのRADIUSサーバのIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
```

#### 802.1x認証の有効化
[**YAMAHA SWXのポート**](#yamaha-swxのポート)に従い、802.1x認証の設定を行います。

```
(config)#aaa authentication dot1x
(config)#aaa authentication auth-mac
(config)#auth-mac auth-user unformatted lower-case
(config)#interface <802.1x認証の有効化する物理ポート名（例：ポート1.1～ポート1.6の物理ポートの場合、`port1.1-6`のように指定する。）>
(config-if)#dot1x port-control auto
(config-if)#auth host-mode multi-supplicant
(config-if)#auth-mac enable
(config-if)# auth dynamic-vlan-creation
```

#### 設定を保存
```
#save
```

#### サンプルコンフィグ
[ダウンロード](./networkauth-yamaha_swx-switch-sampleconfig-mab-dvlan.txt){ target=_blank .md-button .md-button--primary }

## 動作確認方法
### ネットワーク接続の認証（PEAP(MSCHAPv2)方式のパスワード認証）
#### ネットワークへ接続（Windows 11 の場合）
1. PCのネットワークインターフェースの802.1x認証を有効化する必要があります。Windowsの**サービス設定**画面から**Wired AutoConfig**サービスをダブルクリックします。

    [![Screenshot](/images/2022-09-06_15-40-20.png)](/images/2022-09-06_15-40-20.png)

2. PC起動時に、ネットワークインターフェースの802.1x認証を有効化するために、**Wired AutoConfig**サービスの**スタートアップの種類**を**自動**に変更します。そして、**開始**ボタンをクリックして、802.1x認証を有効化します。

    [![Screenshot](/images/2022-09-06_15-40-39.png)](/images/2022-09-06_15-40-39.png)

3. PCをYAMAHA SWXの802.1x認証を有効にしたポート（[YAMAHA SWXのポート](#yamaha-swxのポート)の**802.1x認証の有効化**を参照）へ接続します。

4. ログイン要求がポップアップします。**サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-00-04.png)](/images/2022-09-06_17-00-04.png)

5. **認証設定**の**編集**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_18-15-32.png)](/images/2022-09-06_18-15-32.png)


6. 以下を設定し、**保存**ボタンをクリックします。
    
    | 設定項目 | 設定内容 |
    | :--- | :--- |
    | EAPメソッド | 保護されたEAP(PEAP) |
    | 認証方法 | セキュリティで保護されたパスワード(EAP-MSCHAP v2) |

    [![Screenshot](/images/2022-09-06_18-16-26.png)](/images/2022-09-06_18-16-26.png)

7. **サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-22-12.png)](/images/2022-09-06_17-22-12.png)

8. **SingleIDのユーザ**[参照](#ユーザの情報)の認証情報を入力し、**OK**ボタンをクリックします。

    [![Screenshot](/images/2022-09-06_15-44-57.png)](/images/2022-09-06_15-44-57.png)

9. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | [RADIUSの情報](#radiusの情報)の**RADIUSサーバのホスト名** |
    | **発行元** | R3 |

    [![Screenshot](/images/2022-09-06_15-57-08.png)](/images/2022-09-06_15-57-08.png)

10. 接続成功したことを確認します。

    [![Screenshot](/images/2022-09-06_17-27-56.png)](/images/2022-09-06_17-27-56.png)

    !!! info
        YAMAHA SWXで`#show auth supplicant`コマンドを実行して、接続状況を確認できます。
