# RADIUS認証ログのエラー確認
文書更新日:2026-09-01

RADIUS認証に失敗した場合は、まず**SingleID 管理者ポータル＞ログ＞RADIUS認証ログ**画面で、認証を試行した時刻のログを確認してください。

RADIUS認証ログでは、SingleIDが認証要求を処理した結果を確認できます。利用端末またはネットワーク機器に表示されるエラーだけでは、原因を特定できない場合があります。

本ページでは、SingleIDで確認した代表的なRADIUS認証ログのエラーと確認観点を掲載しています。すべてのエラーメッセージを網羅するものではありません。

!!! info "ログ表示に関する注意"
    RADIUS認証ログが表示されるまで、認証実行後から5分程度かかる場合があります。認証直後にログが表示されない場合は、少し時間をおいてから再度確認してください。

## 認証結果の一覧

| パケットタイプ | 認証タイプ | エラーメッセージ | 考えられる原因 | 確認・対応 |
| --- | --- | --- | --- | --- |
| `Access-Accept` | `Accept/-`、`eap_xxxxxxxx/MD5`、`eap_xxxxxxxx/TTLS`、`otp_xxxxxxxx/-` | `-/-` | MABまたはOTPを含む認証成功 | 認証後の接続に失敗する場合は、RADIUSクライアント側の認可設定、アドレス払い出し、接続先設定を確認します。 |
| `Access-Accept` | `PAP/-`、`CHAP/-`、`MS-CHAP/-`、`eap_xxxxxxxx/MSCHAPv2`、`eap_xxxxxxxx/PEAP`、`eap_xxxxxxxx/TLS`、`eap_xxxxxxxx/TTLS` | `-/The user account was found.` | 通常のユーザ認証成功 | 認証自体は成功しています。RADIUSクライアント側の後続処理を確認します。 |
| `Access-Reject` | `otp_xxxxxxxx/-` | `-/-` | OTPまたはパスワードが誤り | パスワードと6桁のOTPを `パスワード:123456` の形式で入力していること、端末時刻が正しいことを確認します。 |
| `Access-Reject` | `otp_xxxxxxxx/-` | `otp_xxxxxxxx: Server returned:/-` | ユーザ無効、OTP未登録、またはOTP入力形式不正 | ユーザが有効であること、OTPが登録済みであること、入力形式を確認します。 |
| `Access-Reject` | `PAP/-` | `No Auth-Type found: rejecting the user via Post-Auth-Type = Reject/The user account was not found.` | ユーザが見つからない | ユーザIDを確認します。 |
| `Access-Reject` | `-/-` | `No Auth-Type found: rejecting the user via Post-Auth-Type = Reject/The user account was found.` | PAPパスワードが空 | 利用端末またはネットワーク機器でパスワードが設定されていることを確認します。 |
| `Access-Reject` | `PAP/-` | `pap: Cleartext password does not match "known good" password/The user account was found.` | ユーザパスワード不正。または、Message-Authenticatorを含まない要求ではRADIUS共有シークレット不一致 | ユーザパスワードとRADIUSクライアントに保存された資格情報を確認します。Message-Authenticatorを送信しない機器では、RADIUS共有シークレットも確認します。 |
| `Access-Reject` | `CHAP/-` | `chap: &control:Cleartext-Password is required for authentication/The user account was not found.` | ユーザが見つからない | ユーザIDを確認します。 |
| `Access-Reject` | `CHAP/-` | `chap: Password comparison failed: password is incorrect/The user account was found.` | パスワード不正 | パスワードを確認します。 |
| `Access-Reject` | `MS-CHAP/-` | `mschap: FAILED: No NT-Password.  Cannot perform authentication/The user account was not found.` | ユーザが見つからない | ユーザIDを確認します。 |
| `Access-Reject` | `MS-CHAP/-` | `mschap: MS-CHAP2-Response is incorrect/The user account was found.` | パスワード不正 | パスワードを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/MD5` | `eap_md5: Cleartext-Password is required for EAP-MD5 authentication/-` | MABのMACアドレスが未登録、またはユーザ名とMACアドレスが不一致 | MACアドレスとMABルールを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/MD5` | `-/-` | 登録済みMACアドレスに対するEAP-MD5パスワード不正 | MABのMACアドレスと、機器が送信するEAP-MD5パスワードを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/MSCHAPv2` | `mschap: FAILED: No NT-Password.  Cannot perform authentication/-` または `mschap: FAILED: No NT-Password.  Cannot perform authentication/The user account was not found.` | ユーザ未登録、またはサイト条件により認証情報を取得できない | ユーザIDとRADIUSサイトの条件を確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/MSCHAPv2` | `mschap: MS-CHAP2-Response is incorrect/The user account was found.` | パスワード不正 | パスワードを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/PEAP` | `eap_peap: The users session was previously rejected: returning reject (again.)/The user account was found.` または `eap_peap: The users session was previously rejected: returning reject (again.)/The user account was not found.` | EAP内側の認証が拒否された | 後半のメッセージに従い、ユーザIDまたはパスワードを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) TLS - Alert read:fatal:internal error/-` | 利用端末のRADIUSサーバ名またはドメインの検証条件が一致しない | 利用端末のRADIUSサーバ名またはドメインに、SingleID 管理者ポータルの **認証＞RADIUS＞基本情報** に表示されるホスト名を設定します。 |
| `Access-Reject` | `eap_xxxxxxxx/TTLS` | `eap_xxxxxxxx: Failed continuing EAP TTLS (21) session.  EAP sub-module failed/-`、`eap_xxxxxxxx: Failed continuing EAP TTLS (21) session.  EAP sub-module failed/The user account was found.`、または `eap_xxxxxxxx: Failed continuing EAP TTLS (21) session.  EAP sub-module failed/The user account was not found.` | EAP内側の認証またはOTP認証が拒否された | 後半のメッセージに従い、ユーザID、パスワード、OTPを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `-/The user account was not found.` | 証明書CNに対応するユーザが見つからない | 証明書CNとSingleIDのユーザIDを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) TLS - Alert read:fatal:unknown CA/-` | 利用端末で、RADIUSサーバ証明書の検証に使用するCAが正しく設定されていない | 利用端末で、SingleIDのRADIUSサーバ証明書を検証できるCAを設定します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: Program returned code (2) ... verification failed/-` | クライアント証明書が失効している、またはSingleIDが信頼していない発行元の証明書 | 証明書の失効状態、発行元、証明書チェーンを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) OpenSSL says error 18 : self signed certificate/-` | 自己署名のクライアント証明書 | SingleIDが信頼する発行元のクライアント証明書を使用します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) OpenSSL says error 20 : unable to get local issuer certificate/-` | SingleIDが信頼していない発行元のクライアント証明書 | クライアント証明書の発行元と証明書チェーンを確認します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) OpenSSL says error 19 : self signed certificate in certificate chain/-` | クライアント証明書チェーンに自己署名証明書が含まれる | SingleIDが信頼する発行元のクライアント証明書を使用します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) OpenSSL says error 10 : certificate has expired/-` | クライアント証明書の有効期限切れ | 有効なクライアント証明書を使用します。 |
| `Access-Reject` | `eap_xxxxxxxx/TLS` | `eap_tls: (TLS) OpenSSL says error 9 : certificate is not yet valid/-` | クライアント証明書がまだ有効開始前 | 有効開始日を過ぎたクライアント証明書を使用します。 |
| `Access-Reject` | `-/-`、`CHAP/-`、`MS-CHAP/-`、`eap_xxxxxxxx/Identity`、`eap_xxxxxxxx/TLS` | `-/The user account is locked.` | ユーザが無効 | 管理者ポータルで対象ユーザを有効にします。 |
| `Access-Reject` | `eap_xxxxxxxx/PEAP` | `eap_peap: The users session was previously rejected: returning reject (again.)/The user account is locked.` | ユーザが無効 | 管理者ポータルで対象ユーザを有効にします。 |
| `Access-Reject` | `eap_xxxxxxxx/TTLS` | `eap_xxxxxxxx: Failed continuing EAP TTLS (21) session.  EAP sub-module failed/The user account is locked.` | ユーザが無効 | 管理者ポータルで対象ユーザを有効にします。 |
| `Access-Reject` | `Reject/-`、`Reject/MSCHAPv2`、`Reject/PEAP`、`Reject/TTLS`、`Reject/TLS` | `Rejected: The reject rule is matched./The user account was found.` または `Rejected: The reject rule is matched./-` | RADIUSサイトの拒否ルールに一致 | 許可ユーザ、許可グループ、条件、ルール順序、NAS ID、SSID、NASポートタイプを確認します。 |
| `Access-Reject` | `-/-` | `Rejected: There is no sites assosiated with sent NAS-IP-Address/NAS-Identifier./-` | 拡張RADIUSで、サイト識別の属性または属性値が、RADIUSクライアントから送信されたNAS-IP-AddressまたはNAS-Identifierと一致しない | サイトの識別属性と属性値を確認します。NAS-Identifierを使う場合は、安定した共通文字列を部分一致条件として設定できるか確認します。 |
| `Access-Reject` | `-/-` | `Rejected: User-Name contains whitespace/-` | ユーザIDに空白文字がある | 利用端末またはRADIUSクライアントのユーザID設定から空白を除去します。 |
| `Access-Reject` | `-/-` | `Rejected: Multiple @ in User-Name/-`、`Rejected: Realm begins with a dot/-`、`Rejected: Realm ends with a dot/-`、`Rejected: User-Name contains multiple ..s/-` | ユーザIDまたはrealmの形式が不正 | ユーザIDの形式を確認します。 |

## ログ項目の見方

| 項目 | 確認内容 |
| --- | --- |
| パケットタイプ | RADIUS の最終応答です。`Access-Accept` は成功、`Access-Reject` は拒否です。 |
| 認証タイプ | SingleIDが処理した認証方式です。`PAP/-`、`CHAP/-`、`MS-CHAP/-`、`eap_xxxxxxxx/TLS`、`otp_xxxxxxxx/-` などが表示されます。`xxxxxxxx` の数字部分は環境によって異なります。 |
| エラーメッセージ | `/` の前後を含めて表示されます。`-` は該当するメッセージがないことを表します。 |
| RADIUSクライアントIP、NAS ID、SSID | 認証要求を送信した機器・接続先を特定するための情報です。想定した機器・サイトからの認証要求か確認してください。 |

エラーメッセージが `-/-` の場合は、追加のメッセージが記録されていないことを示します。`-/-` 自体は成功を意味しません。パケットタイプと認証タイプをあわせて判断してください。

## RADIUS認証ログがない場合

該当時刻にRADIUS認証ログがない場合は、SingleIDが認証結果を記録する段階までRADIUS認証処理が進んでいない可能性があります。

まず、RADIUSクライアントとなるネットワーク機器から、RADIUS認証要求が送信されているかを確認してください。

### RADIUS認証処理が開始または完了していない

利用端末とネットワーク機器の間で、RADIUS認証を開始する前の処理に失敗している場合があります。

たとえば、次のような原因が考えられます。

* Wi-Fiへの接続や802.1X認証の開始前に失敗している
* 利用端末の802.1X、EAP、証明書などの設定に問題がある
* VPNで、IKE/IPsecやL2TPなど、RADIUS認証より前の接続処理に失敗している
* VPNクライアントとネットワーク機器で、PAP、CHAP、MS-CHAPv2などの認証方式が一致していない
* ネットワーク機器側でRADIUS認証を使用する設定になっていない
* 認証対象となるSSID、VPN、ポートなどにRADIUS設定が適用されていない

この場合、SingleIDにRADIUS認証要求が送信されないか、一連の認証処理が完了しないため、RADIUS認証ログが記録されないことがあります。

ネットワーク機器の認証ログ、システムログ、デバッグログなどを確認し、RADIUS Access-Requestが送信されているか、応答後に認証処理が中断していないか確認してください。

### RADIUS認証要求を送信しているがSingleIDまで到達していない

ネットワーク機器からRADIUS認証要求が送信されている場合は、次の項目を確認してください。

* ネットワーク機器に設定されているRADIUSサーバのアドレスおよび認証ポートが正しいこと
* ファイアウォール、ルータ、NAT、アクセス制御などによりRADIUS通信が遮断されていないこと

#### 標準RADIUSサーバを利用している場合

上記に加えて、SingleIDから見えるRADIUSパケットの送信元グローバルIPアドレスが、SingleIDに登録しているRADIUSクライアントのIPアドレスと一致していることを確認してください。

### SingleIDに到達した後に認証ログが記録される前に破棄されている

RADIUS認証要求がSingleIDまで到達していても、RADIUS共有シークレットの不一致などにより、認証ログが記録される前に要求が破棄される場合があります。

RADIUS共有シークレットが、ネットワーク機器側とSingleID側で一致していることを確認してください。

EAPを利用する場合や、ネットワーク機器がMessage-Authenticator属性を付与する場合、共有シークレットが一致していないとRADIUS認証要求が破棄され、認証ログが記録されないことがあります。

また、一部のネットワーク機器では、共有シークレットに使用する特殊文字の扱いによって、設定した文字列と実際に使用される共有シークレットが一致しない場合があります。共有シークレットの不一致が疑われる場合は、十分な長さの半角英数字のみの文字列に変更して確認してください。

## 問い合わせ時に必要な情報

原因調査が必要な場合は、次の情報を添えて問い合わせてください。

- 対象ユーザID
- 認証を試行した日時とタイムゾーン
- RADIUS認証ログの該当行、または該当ログがないこと
- RADIUSクライアントIP、NAS ID、SSID、認証タイプ
- 利用端末の種類、OS、RADIUS認証の設定内容
- RADIUSクライアントの機器名、ソフトウェアバージョン、構成概要
- 利用端末またはRADIUSクライアントに表示されたエラー内容
- 特定の端末、回線、時間帯でのみ発生するか

パスワード、ワンタイムパスワード、RADIUS共有シークレット、秘密鍵、証明書ファイルは問い合わせに記載または添付しないでください。
