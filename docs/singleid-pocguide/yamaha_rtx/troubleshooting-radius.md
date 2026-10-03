# YAMAHA RTX リモートアクセスVPN 2要素認証のトラブルシューティング
文書更新日:2026-10-03

YAMAHA RTXとSingleIDのRADIUSサーバを連携し、パスワードとOTPによる2要素認証を使用するリモートアクセスVPNに接続できない場合の切り分け方法を説明します。本ページでは、Windows標準VPNクライアントを使用します。

## トラブルシューティング前の準備

現在のログ出力設定を記録してから、YAMAHA RTXで次のコマンドを実行し、IKEとL2TPの詳細ログを有効にしてVPN接続を再試行します。`<gateway_id>`には、対象のセキュリティ・ゲートウェイの識別子を指定してください。複数のセキュリティ・ゲートウェイを使用している場合は、対象となるすべての`gateway_id`に`ipsec ike log`を設定します。

```text
syslog debug on
l2tp syslog on
ipsec ike log <gateway_id> message-info payload-info
```

DEBUGログは大量に出力されるため、確認後はトラブルシューティング前の設定へ戻してください。今回の確認のために各設定を有効にした場合は、次のコマンドで無効化します。複数の`gateway_id`に設定した場合は、それぞれに`no ipsec ike log`を実行してください。

```text
no ipsec ike log <gateway_id>
l2tp syslog off
syslog debug off
```

## SingleIDの認証結果の切り分け

番号付きのボックスをクリックすると、それぞれの詳細フローチャートへ移動します。

<div style="width: 100%; aspect-ratio: 3300 / 1000;">
  <object data="/images/yamaha-rtx-radius-troubleshooting-overview.svg" type="image/svg+xml" aria-label="YAMAHA RTX リモートアクセスVPN 2要素認証の切り分けフロー" style="width: 100%; height: 100%;">
    <a href="/images/yamaha-rtx-radius-troubleshooting-overview.svg">YAMAHA RTX リモートアクセスVPN 2要素認証の切り分けフロー</a>
  </object>
</div>

!!! info
    SingleIDのRADIUS認証ログが表示されるまで、認証実行後から5分程度かかる場合があります。認証直後にログが表示されない場合は、少し時間をおいてから再度確認してください。

    サイト識別失敗ログだけが表示される場合は、対象ユーザの認証結果ログがないものとして、**1. 対象ユーザの認証結果ログがない**へ進んでください。

## 対象ユーザの認証結果ログがない場合

SingleIDに対象ユーザの認証結果ログがない場合は、VPN接続のどの段階で処理が止まっているかを確認します。

<a id="case-1"></a>

### 1. 対象ユーザの認証結果ログがない

[![対象ユーザの認証結果ログがない場合のフローチャート](/images/yamaha-rtx-radius-troubleshooting-case-1.svg)](/images/yamaha-rtx-radius-troubleshooting-case-1.svg)

対象ユーザの認証結果ログがない場合は、YAMAHA RTXのログに記録されたメッセージを確認し、IKE、L2TP、PPPユーザ認証、RADIUSの順に処理状況を確認します。

YAMAHA RTXで`show log reverse`を実行し、接続を試行した時刻のログから処理が止まった段階を確認します。

| YAMAHA RTXのログ | 切り分けと確認内容 |
| :--- | :--- |
| 接続試行時刻に`[IKE]`を含むログがない | IKE接続試行の到達を確認できません。VPNクライアント、通信経路、YAMAHA RTXの順に確認します。 |
| `[IKE]`を含むログはあるが、`[L2TP] TUNNEL[...] connected from ...`がない | L2TP接続要求の受信を確認できません。VPNクライアント、通信経路、YAMAHA RTXの順に確認します。 |
| `[L2TP] TUNNEL[...] connected from ...`はあるが、`tunnel ... established`または`session ... established`がない | L2TPセッションの確立を確認できません。VPNクライアントとYAMAHA RTXを確認します。 |
| `[L2TP] TUNNEL[...] session ... established`はあるが、`PP[ANONYMOUSxx] Call detected from user '...'`がない | PPPユーザ認証の開始を確認できません。VPNクライアントとYAMAHA RTXを確認します。 |

クライアント側の設定と通信環境に問題がない場合は、ログで処理が止まった段階に対応するYAMAHA RTXのIPsec、L2TP、トンネル、同時接続数、PPP認証方式、匿名接続設定を確認します。

#### ローカルユーザ

接続時のユーザ名が`pp auth username`に登録されている場合、YAMAHA RTXのローカル認証が使用されます。ローカルパスワードが一致しない場合もSingleIDのRADIUS認証へフォールバックせず、認証に失敗します。同じユーザ名をYAMAHA RTXとSingleIDの両方に登録している場合は、SingleIDに対象ユーザの認証結果ログが記録されません。

#### サイト識別失敗ログ

拡張RADIUSサーバを使用している場合は、同じ時刻に次のRADIUS認証ログが記録されていないか確認します。

| パケットタイプ | 認証タイプ | エラーメッセージ |
| :--- | :--- | :--- |
| `Access-Reject` | `-/-` | `Rejected: There is no sites assosiated with sent NAS-IP-Address/NAS-Identifier./-` |

このログがある場合、YAMAHA RTXが送信したNAS-IP-Address属性の値は、ログの**NAS-IP**に表示されます。RADIUSサイトの**サイト識別する属性**が**NAS-IP-Address**、**属性値**がログの**NAS-IP**と一致していることを確認してください。

#### YAMAHA RTXのRADIUS送信先設定

YAMAHA RTXで`show config`を実行し、次の設定を確認します。

```text
radius auth on
radius auth server <RADIUSサーバのプライマリIPアドレス> <RADIUSサーバのセカンダリIPアドレス>
radius auth port <RADIUSサーバのポート番号>
```

各設定値は、次の内容と一致させます。

| 確認項目 | 確認内容 |
| :--- | :--- |
| RADIUSサーバのIPアドレス | **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブに表示される、使用するRADIUSサーバのプライマリおよびセカンダリIPアドレス |
| RADIUSサーバのポート番号 | 標準RADIUSサーバの場合は、RADIUSサイトで選択した**サーバ番号**に対応するRADIUSポート番号です。拡張RADIUSサーバの場合は、拡張RADIUSサーバに割り当てられたRADIUSポート番号です。 |

#### SingleIDのRADIUS設定

標準RADIUSサーバを使用している場合は、**SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブで、対象のRADIUSサイトについて次の内容を確認します。

* **有効/無効**が**有効**であること
* **サーバ**が**標準**であること
* **サーバ番号**がYAMAHA RTXに設定したRADIUSポート番号と対応していること
* **IP or ホスト名**がSingleIDから見えるRADIUSパケットの送信元グローバルIPアドレスと一致していること

拡張RADIUSサーバを使用している場合は、次の内容を確認します。

* **SingleID 管理者ポータル＞認証＞RADIUS＞基本情報**タブで、YAMAHA RTXに設定したRADIUSポート番号の拡張RADIUSサーバが登録されていること
* **SingleID 管理者ポータル＞認証＞RADIUS＞簡易設定**タブで、対象のRADIUSサイトの**有効/無効**が**有効**であること
* **サーバ**が**拡張**であること
* 登録した拡張RADIUSサーバの**サーバ番号**が選択されていること

#### 通信経路

YAMAHA RTXからRADIUSサーバのプライマリおよびセカンダリIPアドレスへ、設定したUDPポートで通信できることを確認します。上位のファイアウォールやアクセス制御がある場合は、RADIUS通信が遮断されていないことを確認してください。

## 対象ユーザの認証結果ログがある場合

RADIUS認証ログの**パケットタイプ**、**認証タイプ**、**エラーメッセージ**を組み合わせて確認します。

<a id="case-2"></a>

### 2. RADIUS認証ログの確認

| パケットタイプ | 認証タイプ | エラーメッセージ | 判断 | 確認内容 |
| :--- | :--- | :--- | :--- | :--- |
| `Access-Accept` | `otp_xxxxxxxx/-` | `-/-` | 2要素認証に成功 | VPNに接続できない場合は、YAMAHA RTXの後続処理を確認します。 |
| `Access-Accept` | `PAP/-` | `-/The user account was found.` | パスワード認証に成功し、OTPは処理されていない | パスワード欄へ6桁のOTPを入力していること、`パスワード:123456`の形式になっていること、RADIUSサイトの**2要素認証（OTP）のみ許可**が有効であることを確認します。 |
| `Access-Reject` | `otp_xxxxxxxx/-` | `-/-` | パスワードまたはOTPが不正 | パスワードと6桁のOTPを再入力し、OTPを表示する端末の時刻を確認します。 |
| `Access-Reject` | `otp_xxxxxxxx/-` | `otp_xxxxxxxx: Server returned:/-` | ユーザ無効、OTP未登録、またはOTP入力形式不正 | ユーザの状態、OTP登録、パスワード欄の入力形式を確認します。 |
| `Access-Reject` | `PAP/-` | `pap: Cleartext password does not match "known good" password/The user account was found.` | OTP認証として処理されず、PAPパスワードが不一致。または、RADIUS共有シークレットが不一致 | 通常パスワードが正しいことと、パスワード欄へ`パスワード:123456`の形式で6桁のOTPを入力していることを確認します。入力内容が正しい場合は、標準RADIUSサーバではYAMAHA RTXとRADIUSサイト、拡張RADIUSサーバではYAMAHA RTXと拡張RADIUSサーバに設定したRADIUS共有シークレットが一致していることを確認します。改善しない場合は、両方のRADIUS共有シークレットを同じ英数字のみの文字列へ変更して再度確認します。 |
| 上記以外の組み合わせ | （組み合わせによる） | （組み合わせによる） | 想定している認証結果と異なる | 認証タイプとエラーメッセージを[RADIUS認証ログのエラー確認](../../singleid-adminguide/radius_authlog_errors.md)と照合します。 |

`xxxxxxxx`の数字部分は環境によって異なります。エラーメッセージの`-`は、該当するメッセージがないことを示します。

## 問い合わせ時に必要な情報

原因を特定できない場合は、次の情報を添えてお問い合わせください。

* 対象ユーザID
* 認証を試行した日時とタイムゾーン
* YAMAHA RTXの機種名およびファームウェアリビジョン
* 該当時刻のYAMAHA RTXのログ
* YAMAHA RTXのリモートアクセスVPNおよびRADIUS設定内容

パスワード、ワンタイムパスワード、RADIUSクライアントのシークレット、IPsec事前共有鍵は記載しないでください。

## 参考資料

* [RADIUSによる認証を使用するか否かの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/radius/radius_auth.html){ target=_blank }
* [L2TP/IPsec（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/docs/l2tp_ipsec/){ target=_blank }
* [L2TPのコネクション制御のSYSLOGを出力するか否かの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/l2tp/l2tp_syslog.html){ target=_blank }
* [IKEのログの種類の設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/ipsec/ipsec_ike_log.html){ target=_blank }
* [DEBUGタイプのSYSLOGを出力するか否かの設定（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/setup/syslog_debug.html){ target=_blank }
* [ログの表示（YAMAHA）](https://www.rtpro.yamaha.co.jp/RT/manual/rt-common/logging/show_log.html){ target=_blank }
