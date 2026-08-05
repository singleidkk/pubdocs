# 既存のYAMAHA RTXリモートアクセスVPNに2要素認証を設定する
文書更新日:2026-08-05

## 目的
* YAMAHA RTXのL2TP/IPsecリモートアクセスVPNをローカルユーザ認証で運用している環境に、SingleIDの2要素認証を設定します。
* 既存のリモートアクセスVPN設定のうち、変更が必要な箇所だけを説明します。
* SingleID側の設定は、使用するRADIUSサーバに応じて、[標準RADIUSサーバの設定方法](vpn-group-otp.md)または[拡張RADIUSサーバの設定方法](vpn-group-otp-adv.md)を参照します。

!!! info
    本番環境で事前に検証する場合は、YAMAHA RTXのローカルユーザに登録されていない検証用ユーザをSingleIDに作成し、ローカルユーザを削除せずに確認します。設定変更は既存ユーザにも影響するため、影響の少ない時間帯に実施してください。検証だけで終了する場合は、認証方式とRADIUS認証を検証前の設定へ戻します。

## 認証先の動作

`radius auth on`を設定すると、YAMAHA RTXのローカル認証とSingleIDのRADIUS認証を併用できます。認証先は、接続時のユーザ名により次のように決まります。

| **状態** | **認証先と動作** |
| :-- | :-- |
| ユーザ名が`pp auth username`に登録されている | YAMAHA RTXでローカル認証します。 |
| ユーザ名が`pp auth username`に登録されていない | SingleIDへRADIUS認証を行います。 |
| 同じユーザ名がYAMAHA RTXとSingleIDの両方に登録されている | YAMAHA RTXのローカル認証を使用します。 |
| YAMAHA RTXに登録されたユーザのローカルパスワードが一致しない | SingleIDへフォールバックせず、認証に失敗します。 |

## 設定方法

### YAMAHA RTXのRADIUSサーバの設定

1. 設定変更前に、YAMAHA RTXの設定情報をバックアップします。バックアップ方法は、使用する機種の[YAMAHA公式マニュアル](https://network.yamaha.com/support/download){ target=_blank }を参照してください。
2. SSHまたはTelnetで、ローカル管理者でYAMAHA RTXのCLIにログインします。
3. コマンドで以下を設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバのプライマリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの、使用するRADIUSサーバの**IPアドレス＞プライマリ**です。 |
    | **RADIUSサーバのセカンダリIPアドレス** | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブの、使用するRADIUSサーバの**IPアドレス＞セカンダリ**です。 |
    | **RADIUSサーバのポート番号** | 標準RADIUSサーバの場合は、RADIUSサイトで選択した**サーバ番号**に対応するポート番号です。拡張RADIUSサーバの場合は、拡張RADIUSサーバに割り当てられたポート番号です。 |
    | **RADIUSクライアントのシークレット** | 標準RADIUSサーバの場合は、RADIUSサイトの**シークレット**に設定した文字列です。拡張RADIUSサーバの場合は、拡張RADIUSサーバの登録時に設定した**シークレット**です。 |

    ``` linenums="1"
    radius auth on
    radius auth server <RADIUSサーバのプライマリIPアドレス> <RADIUSサーバのセカンダリIPアドレス>
    radius auth port <RADIUSサーバのポート番号>
    radius secret <RADIUSクライアントのシークレット>
    ```

### YAMAHA RTXのリモートアクセスユーザの認証の設定

1. コマンドで、リモートアクセスVPNのユーザ認証方式をPAPに設定します。

    !!! warning
        `pp auth request pap`への変更は、同じリモートアクセスVPN設定を使用する既存ユーザにも影響します。VPNクライアント側でもPAPを許可してください。

    ``` linenums="1"
    pp select anonymous
    pp auth request pap
    ```

### YAMAHA RTXのローカルユーザの削除

1. SingleIDへ移行するユーザを、YAMAHA RTXのローカルユーザから削除します。

    !!! warning
        SingleIDのRADIUSサイトで**2要素認証（OTP）のみ許可**を**有効**にしても、YAMAHA RTXのローカル認証には適用されません。SingleIDへ移行したユーザが`pp auth username`に残っていると、SingleIDの2要素認証を経由せずにローカル認証されます。

        ローカルユーザは切り替え期間だけ残し、切り替え完了後は一般ユーザの`pp auth username`をすべて削除してください。緊急時用のローカルユーザを残す場合は、通常のVPN接続には使用せず、利用条件、パスワードの保管方法、利用後の変更手順を定めてください。

    ``` linenums="1"
    pp select anonymous
    no pp auth username <username>
    ```

### YAMAHA RTXの設定の保存

1. `save`コマンドを実行し、設定を保存します。

    ```
    save
    ```

!!! info
    設定後の動作確認は、使用するRADIUSサーバに応じて、[標準RADIUSサーバの動作確認方法](vpn-group-otp.md#動作確認方法)または[拡張RADIUSサーバの動作確認方法](vpn-group-otp-adv.md#動作確認方法)を参照してください。

## 参考資料

* [RADIUSによる認証を使用するか否かの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/radius/radius_auth.html){ target=_blank }
* [要求する認証タイプの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/ppp/ppp_pp_auth_request.html){ target=_blank }
* [相手の名前とパスワードの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/ppp/pp_auth_username.html){ target=_blank }
* [設定の保存（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/operation/save.html){ target=_blank }
