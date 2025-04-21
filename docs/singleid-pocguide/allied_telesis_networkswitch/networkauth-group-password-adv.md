# 有線LANアクセス-パスワード認証
文書更新日:2025-04-21

## 目的
* SingleIDのユーザで、アライドテレシス ネットワークスイッチへアクセスします。
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

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**アライドテレシス ネットワークスイッチ**の**登録**ボタンをクリックします。**アライドテレシス ネットワークスイッチ**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | **NAS-Identifier**を選択します。 |
    | **属性値** | 任意の文字列を設定します。（例：allied-telesis_networkswitch） |        

5. **ネットワークアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        認証したユーザおよびグループにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

7. **MACアドレス認証バイパス**タブへ移動します。
8. 802.1x認証をサポートしていないデバイスの**MACアドレス**を大文字英数字の**ハイフン区切り**で入力します。（例：00-E1-5C-68-16-04）

    !!! info
        MACアドレスにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

9. **登録**ボタンをクリックします。

### アライドテレシス ネットワークスイッチの設定
1. アライドテレシス ネットワークスイッチにCLIでログインして設定します。
2. 以下の内容で、RADIUSサーバを登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバのプライマリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **RADIUSサーバのセカンダリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **RADIUSサーバのポート番号** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サーバ番号**に対応したポート番号です。 |
    | **RADIUSクライアントのシークレット** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**シークレット**に設定した文字列です。 |
    | **RADIUSグループ名** | 登録したRADIUSサーバをグループ化します。（例: singleid） |

    ``` title="RADIUSサーバの登録"
    awplus(config)# radius-server host <RADIUSサーバのプライマリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    awplus(config)# radius-server host <RADIUSサーバのセカンダリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    ```

    ``` title="RADIUSサーバのグループ化"
    awplus(config)#aaa group server radius <RADIUSグループ名>
    awplus(config-sg)# server <RADIUSサーバのプライマリIPアドレス> auth-port <RADIUSサーバのポート番号>
    awplus(config-sg)# server <RADIUSサーバのセカンダリIPアドレス> auth-port <RADIUSサーバのポート番号>
    ```

3. 以下の内容で、NAS-Identifier属性の値を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **NAS-Identifier属性の値** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**サイトを識別する属性**の**属性値**に設定した文字列です。（例: allied-telesis_networkswitch） |

    ``` title="NAS-Identifier属性の値の設定"
    awplus(config)# auth radius send nas-identifier <NAS-Identifier属性の値>
    ```

4. **802.1x認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **802.1x認証を有効化する物理ポート名** | 802.1x認証を有効化する物理ポート名です。（例: port1.0.1-1.0.16） |
    | **RADIUSグループ名** | [アライドテレシス ネットワークスイッチの設定](#アライドテレシス-ネットワークスイッチの設定)の手順で作成したRADIUSグループです。（例: singleid） |


    ``` title="802.1X認証の設定"
    awplus(config)# aaa authentication dot1x default group <RADIUSグループ名>
    awplus(config)# interface <802.1x認証を有効化する物理ポート名>
    awplus(config-if)# dot1x port-control auto
    awplus(config-if)# auth host-mode single-host
    awplus(config-if)# spanning-tree edgeport
    ```

    !!! Info
        * 認証機能の有効なポートでは認証成功時にスパニングツリープロトコルのトポロジーチェンジが発生します。これを回避するには、該当ポートをエッジポートに設定しておきます
        * この設定例では、Single-Hostモード（単一ホストモード）で設定しています。

5. 必要に応じて、**再認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **再認証を有効化する物理ポート名** | 再認証を有効化する物理ポート名です。（例: port1.0.1-1.0.16） |

    ``` title="再認証の設定"
    awplus(config)# interface <再認証を有効化する物理ポート名>
    awplus(config-if)# auth reauthentication
    awplus(config-if)# auth timeout reauth-period 86400
    ```

    !!! Info
        アライドテレシス スイッチは、認証成功後に再認証は行われません。一定期間で再認証させたい場合には、設定変更が必要です。

6. （MACベース認証を利用する場合）**MACベース認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **MACベース認証を有効化する物理ポート名** | MACベース認証を有効化する物理ポート名です。（例: port1.0.1-1.0.16） |
    | **RADIUSグループ名** | [アライドテレシス ネットワークスイッチの設定](#アライドテレシス-ネットワークスイッチの設定)の手順で作成したRADIUSグループです。（例: singleid） |

    ``` title="MACベース認証の有効化"
    awplus(config)# aaa authentication auth-mac default group <RADIUSグループ名>
    awplus(config)# interface <MACベース認証を有効化する物理ポート名>
    awplus(config-if)# auth-mac enable
    ```

    !!! Info
        MACベース認証時の初期値の認証方式は、PAPです。

7. （MACベース認証を利用する場合）**スパニングツリー**を**無効化**します。

    ``` title="スパニングツリーの無効化"
    awplus(config)# no spanning-tree stp enable
    awplus(config)# no spanning-tree rstp enable
    awplus(config)# no spanning-tree mstp enable
    ```

    !!! Info
        同一ポート上でスパニングツリープロトコルとMACベース認証は併用できないため、スパニングツリープロトコルを無効にします。

8. （MACベース認証を利用する場合）MACベース認証時のユーザ名・パスワードの形式を変更します。

    ``` title="MACベース認証時のユーザ名、パスワードの形式を変更"
    awplus(config)# awplus(config)#auth-mac username unformatted lower-case
    ```

9. （ダイナミックVLANを利用する場合）VLANを作成します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VLAN ID** | ダイナミックVLANで割り当てるVLAN IDです。（例: 101） |

    ``` title="VLANの作成"
    awplus(config)#vlan database
    awplus(config-vlan)#vlan <VLAN ID>
    ```

10. （ダイナミックVLANを利用する場合）アップリンクポートをトランクポートに設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **アップリンクポートのインターフェース名** | アップリンクポートのインターフェース名です。（例: port1.0.24） |
    | **VLAN ID** | ダイナミックVLANで割り当てるVLAN IDです。（例: 101） |

    ``` title="トランクポートの設定"
    awplus(config)#interface <アップリンクポートのインターフェース名>
    awplus(config-if)#switchport mode trunk
    awplus(config-if)#switchport trunk allowed vlan add <VLAN ID>
    ```

11. （ダイナミックVLANを利用する場合）**ダイナミックVLAN**を**有効化**にします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ダイナミックVLANを有効化する物理ポート名** | ダイナミックVLANを有効化する物理ポート名です。（例: port1.0.1-1.0.16） |

    ``` title="ダイナミックVLANの有効化"
    awplus(config)# interface <ダイナミックVLANを有効化する物理ポート名>
    awplus(config-if)# auth dynamic-vlan-creation
    awplus(config-if)# auth timeout connect-timeout 120
    ```

    !!! Info
        アライドテレシス スイッチの設定ガイドに従い、`auth timeout connect-timeout`を初期値から変更します。<br><br>**アライドテレシスの設定ガイドからの抜粋**<br>Note - Web認証、ダイナミックvlan使用時は、auth timeout connect-timeoutコマンドを使用し、デフォルトの30秒より長い時間を設定してください。（例：auth timeout connect-timeout 120）

12. 設定を保存します。
    
    ```
    awplus# write memory
    ```

!!! info
    アライドテレシス ネットワークスイッチの設定については、[**CentreCOM x230シリーズ コマンドリファレンス 5.4.5 : インターフェース / ポート認証**](https://www.allied-telesis.co.jp/support/list/switch/x230/rel/5.4.5-0.4/001990d/docs/overview-31.html){ target=_blank }を参考にさせていただきました。

[AT-x230-28GPのサンプルコンフィグ1 ダウンロード](./networkauth-allied_telesis_networkswitch-sampleconfig.txt){ target=_blank .md-button .md-button--primary }

## 動作確認方法
Windows端末からパスワードによるネットワークアクセスの認証が可能なことを確認します。

### 設定
1. PCのネットワークインターフェースの802.1x認証を有効化する必要があります。Windowsの**サービス設定**画面から**Wired AutoConfig**サービスをダブルクリックします。

    [![Screenshot](/images/2022-09-06_15-40-20.png)](/images/2022-09-06_15-40-20.png)

2. PC起動時に、ネットワークインターフェースの802.1x認証を有効化するために、**Wired AutoConfig**サービスの**スタートアップの種類**を**自動**に変更します。そして、**開始**ボタンをクリックして、802.1x認証を有効化します。

    [![Screenshot](/images/2022-09-06_15-40-39.png)](/images/2022-09-06_15-40-39.png)

3. PCをネットワークスイッチの802.1x認証を有効にしたポートへ接続します。

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

### 接続
1. **サインイン**をクリックします。

    [![Screenshot](/images/2022-09-06_17-22-12.png)](/images/2022-09-06_17-22-12.png)

2. 認証情報を入力し、**OK**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ユーザ名** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順で許可したユーザです。 |
    | **パスワード** | **ユーザのパスワード**です。 |

    [![Screenshot](/images/2022-09-06_15-44-57.png)](/images/2022-09-06_15-44-57.png)

3. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |
    | **発行元** | R3 |

    [![Screenshot](/images/2022-09-06_15-57-08.png)](/images/2022-09-06_15-57-08.png)

4. 接続成功したことを確認します。

    [![Screenshot](/images/2022-09-06_17-27-56.png)](/images/2022-09-06_17-27-56.png)

### MACアドレス認証バイパス
1. **802.1x認証を無効にした**PCをネットワークスイッチの802.1x認証を有効にしたポートへ接続します。
2. １分間ほど待ち、ネットワークへ接続できたことを確認します。

!!! info
    アライドテレシス ネットワークスイッチで`awplus# show dot1x supplicant brief`コマンドを実行して、接続状況を確認できます。

``` title="出力結果のサンプル"
Interface port1.0.1
authenticationMethod: dot1x/mac
totalSupplicantNum: 1
authorizedSupplicantNum: 1
    macBasedAuthenticationSupplicantNum: 0
    dot1xAuthenticationSupplicantNum: 1
    webBasedAuthenticationSupplicantNum: 0
    otherAuthenticationSupplicantNum: 0

Interface   VID  Mode MAC Address    Status            IP Address      Username
=========== ==== ==== ============== ================= =============== ========
port1.0.1   101  D    xxxxx          Authenticated     xxxxx           xxxxx
```
