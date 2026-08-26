# 有線LANアクセス-クライアント証明書認証
文書更新日:2026-08-26

## 目的
* SingleIDのユーザで、YAMAHA SWXシリーズ ネットワークスイッチへアクセスします。
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
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
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

3. **登録**ボタンをクリックします。専用のRADIUSポート番号が割り当てられます。

### SingleIDのRADIUSサイトの登録
1. **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブへ移動します。
2. **カタログ表示**ボタンをクリックします。
3. カタログから**YAMAHA SWXシリーズ**の**登録**ボタンをクリックします。**YAMAHA SWXシリーズ**画面がポップアップします。
4. **基本情報**タブに、以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **有効/無効** | **有効**を選択します。 |
    | **サーバ** | **拡張**を選択します。 |
    | **サーバ番号** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順で登録したサーバ番号を選択します。 |
    | **サイト識別する属性** | **NAS-Identifier**を選択します。 |
    | **属性値** | YAMAHA SWXシリーズ ネットワークスイッチを識別する任意の文字列を設定します。（例：`singleid-yamaha-swx`）<br>ここで設定した文字列を[YAMAHA SWXシリーズ ネットワークスイッチの設定](#yamaha-swxシリーズ-ネットワークスイッチの設定)の手順で設定する**NAS-Identifier**にも設定します。 |

    !!! warning
        NAS-Identifierは部分一致で検索されます。複数のRADIUSサイトを登録する場合は、ネットワーク機器が送信する1つのNAS-Identifierに複数サイトの属性値が一致しないように設定してください。

5. **ネットワークアクセスの認証**タブへ移動します。
6. **許可したいユーザ**および**許可したいグループ**をダブルクリックし、許可へ移動させます。

    !!! info
        * **証明書認証（EAP-TLS）のみ許可**を**有効**にすると、クライアント証明書認証以外の認証方式のアクセスを拒否します。
        * 認証したユーザおよびグループにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

7. （MACアドレス認証バイパスを利用する場合）**MACアドレス認証バイパス**タブへ移動します。
8. （MACアドレス認証バイパスを利用する場合）802.1X認証をサポートしていないデバイスの**MACアドレス**を大文字英数字の**ハイフン区切り**で入力します。（例：00-E1-5C-68-16-04）

    !!! info
        MACアドレスにVLAN IDを割り当てたい場合には、**VLAN ID**入力欄に、割り当てたい**VLAN ID**を入力します。

9. **登録**ボタンをクリックします。

    !!! info
        設定がシステムに反映されるまで最大で15分かかります。

### YAMAHA SWXシリーズ ネットワークスイッチの設定
1. YAMAHA SWXシリーズ ネットワークスイッチにCLIでログインして設定します。
2. SingleIDのクラウドRADIUSと通信するために、デフォルトルートを設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **デフォルトゲートウェイのIPアドレス** | 利用環境のデフォルトゲートウェイのIPアドレスです。 |

    ``` title="デフォルトルートの設定"
    SWX2310(config)# ip route 0.0.0.0/0 <デフォルトゲートウェイのIPアドレス>
    ```

3. 以下の内容で、RADIUSサーバを登録します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバのプライマリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**プライマリ**です。 |
    | **RADIUSサーバのセカンダリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞IPアドレス**の**セカンダリ**です。 |
    | **RADIUSサーバのポート番号** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**のポート番号です。 |
    | **RADIUSクライアントのシークレット** | [SingleIDの拡張RADIUSサーバの登録](#singleidの拡張radiusサーバの登録)の手順の**シークレット**に設定した文字列です。 |

    ``` title="RADIUSサーバの登録"
    SWX2310(config)# radius-server host <RADIUSサーバのプライマリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    SWX2310(config)# radius-server host <RADIUSサーバのセカンダリIPアドレス> auth-port <RADIUSサーバのポート番号> key <RADIUSクライアントのシークレット>
    ```

4. **NAS-Identifier**を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **NAS-Identifier** | [SingleIDのRADIUSサイトの登録](#singleidのradiusサイトの登録)の手順の**属性値**に設定した文字列です。（例：`singleid-yamaha-swx`） |

    ``` title="NAS-Identifierの設定"
    SWX2310(config)# auth radius attribute nas-identifier <NAS-Identifier>
    ```

    !!! info
        初期設定ではNAS-Identifierは送信されません。`auth radius attribute nas-identifier`コマンドを設定すると、RADIUSリクエストでNAS-Identifierが送信されます。

        参考: [NAS-Identifier属性の設定](https://www.rtpro.yamaha.co.jp/RT/manual/swx2310/cmdref/interface/auth/auth_radius_att_nas-identifier.html){ target=_blank }

5. **802.1X認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **802.1X認証を有効化する物理ポート名** | 802.1X認証を有効化する物理ポート名です。（例：`port1.1-6`） |

    ``` title="802.1X認証の設定"
    SWX2310(config)# aaa authentication dot1x
    SWX2310(config)# interface <802.1X認証を有効化する物理ポート名>
    SWX2310(config-if)# dot1x port-control auto
    SWX2310(config-if)# auth host-mode multi-supplicant
    SWX2310(config-if)# auth timeout server-timeout 40
    ```

    !!! info
        YAMAHA公式の計算式に従い、RADIUSサーバ全体の応答待ち時間を40秒に設定します。40秒は、初期値の応答待ち時間5秒、再送回数3回、RADIUSサーバ2台の構成から、`5秒 ×（3回 + 1）× 2台`で算出した値です。

        参考: [RADIUSサーバー全体の応答待ち時間の設定](https://www.rtpro.yamaha.co.jp/RT/manual/swx2310/cmdref/interface/auth/auth_timeout_server-timeout.html){ target=_blank }、[RADIUSサーバー1台あたりの応答待ち時間の設定](https://www.rtpro.yamaha.co.jp/RT/manual/swx2310/cmdref/interface/auth/radius-server_timeout.html){ target=_blank }、[RADIUSサーバーホストの設定](https://www.rtpro.yamaha.co.jp/RT/manual/swx2310/cmdref/interface/auth/radius-server_host.html){ target=_blank }

6. （MACアドレス認証バイパスを利用する場合）**MACベース認証**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **MACベース認証を有効化する物理ポート名** | MACベース認証を有効化する物理ポート名です。（例：`port1.1-6`） |

    ``` title="MACベース認証の設定"
    SWX2310(config)# aaa authentication auth-mac
    SWX2310(config)# auth-mac auth-user unformatted lower-case
    SWX2310(config)# interface <MACベース認証を有効化する物理ポート名>
    SWX2310(config-if)# auth-mac enable
    ```

7. （ダイナミックVLANを利用する場合）VLANを作成します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **VLAN ID** | ダイナミックVLANで割り当てるVLAN IDです。（例：101） |

    ``` title="VLANの作成"
    SWX2310(config)# vlan database
    SWX2310(config-vlan)# vlan <VLAN ID>
    ```

8. （ダイナミックVLANを利用する場合）アップリンクポートをトランクポートに設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **アップリンクポートのインターフェース名** | アップリンクポートのインターフェース名です。（例：`port1.8`） |
    | **VLAN ID** | ダイナミックVLANで割り当てるVLAN IDです。（例：101） |

    ``` title="トランクポートの設定"
    SWX2310(config)# interface <アップリンクポートのインターフェース名>
    SWX2310(config-if)# switchport mode trunk
    SWX2310(config-if)# switchport trunk allowed vlan add <VLAN ID>
    ```

9. （ダイナミックVLANを利用する場合）**ダイナミックVLAN**を**有効化**します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ダイナミックVLANを有効化する物理ポート名** | ダイナミックVLANを有効化する物理ポート名です。（例：`port1.1-6`） |

    ``` title="ダイナミックVLANの設定"
    SWX2310(config)# interface <ダイナミックVLANを有効化する物理ポート名>
    SWX2310(config-if)# auth dynamic-vlan-creation
    ```

10. 設定を保存します。

    ```
    SWX2310# save
    ```

!!! info
    * YAMAHA SWXシリーズ ネットワークスイッチの設定については、[**SWX2310 コマンドリファレンス：ポート認証機能**](https://www.rtpro.yamaha.co.jp/SW/docs/swx2310/Rev_2_04_09/interface/interface_port_auth.html?category=interface&rev=2.04.09){ target=_blank }を参考にさせていただきました。
    * 拡張RADIUSサーバを利用する場合は、サンプルコンフィグにNAS-Identifierの設定を追加してください。

[SWX2310-10Gのサンプルコンフィグ ダウンロード](./networkauth-yamaha_swx-switch-sampleconfig-mab-dvlan.txt){ target=_blank .md-button .md-button--primary }

## 動作確認方法
以下を参考にクライアントを設定して動作確認します。

[クライアント接続設定](../clients/index.md)
