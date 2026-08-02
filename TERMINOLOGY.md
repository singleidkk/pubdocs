# 表記ルール

このファイルは、SingleIDドキュメントで使用する主な用語の表記ルールをまとめた、ドキュメント執筆者向けのガイドです。文書の構成やMarkdownの記述方法については、[CONTRIBUTING.md](CONTRIBUTING.md)を参照してください。

## 基本方針

* SingleID の機能、画面、データを説明する場合は、SingleID 側の用語を優先します。
* Microsoft、Apple、Google など外部サービスの画面名、ボタン名、公式名称を引用する場合は、外部サービス側の表記を優先します。
* 画面名やボタン名は、可能な限り実際の UI 表記に合わせます。

## ユーザ / ユーザー

| 表記 | 使用する場面 |
| --- | --- |
| ユーザ | SingleID 上の利用者、SingleID 側の画面、SingleID 側のデータを指す場合 |
| ユーザー | Microsoft Entra ID など外部サービスの画面名、公式用語、UI 表記を指す場合 |

例:

* SingleID のユーザ
* SingleID ユーザポータル
* Microsoft Entra ID のユーザー プリンシパル名（UPN）
* Microsoft Entra ID の **ユーザーとグループ** 画面

## ポータル

| 表記 | 使用する場面 |
| --- | --- |
| SingleID 管理者ポータル | SingleID の管理者向け画面 |
| SingleID MSP管理者ポータル | 特権MSP、MSP、サポートアカウントの管理者向け画面 |
| ユーザポータル | SingleID の利用者向け画面 |
| 管理者ユーザポータル | 管理者自身のパスワード変更や OTP 設定を行う画面 |

## Microsoft Entra ID

* 初出から `Microsoft Entra ID` を使用します。
* 旧称を補足する必要がある場合のみ、`Microsoft Entra ID（旧 Azure Active Directory）` と記載します。
* `EntraID`、`Azure AD`、`Azure Active Directory` は、画面名や引用を除き、新規文書では使用しません。

## Microsoft Entraの証明書認証

| 表記 | 使用する場面 |
| --- | --- |
| Microsoft Entra CBA | Microsoft Entra IDの証明書ベース認証を本文で説明する場合 |
| 証明書ベースの認証 | Microsoft Entra 管理センターの項目名を記載する場合 |
| SingleIDクライアント証明書 | SingleIDが発行したクライアント証明書を明示する場合 |
| Microsoft Entra 2要素認証（パスワード＋SingleIDクライアント証明書） | パスワードとSingleIDクライアント証明書を組み合わせる構成のページタイトルまたはリンク名 |

証明書一般を説明する場合は、`クライアント証明書`を使用します。Microsoftなど外部サービスのページ名やUI表記を引用する場合は、外部サービス側の表記を優先します。

## 2要素認証

* `2要素認証` を使用します。
* `２要素認証`、`二要素認証` は、引用や画面名を除き使用しません。

## IPsec

* 説明文では `IPsec` を使用します。
* 外部サービスや製品画面の項目名として `IPSec` が表示される場合は、画面名に合わせます。

## RADIUSサイト

* SingleID の RADIUS サイトを指す場合は `RADIUSサイト` を使用します。
* `RADIUS サイト` のように、`RADIUS` と `サイト` の間にスペースは入れません。

## 通知メール

| 表記 | 使用する場面 |
| --- | --- |
| ユーザの登録通知 | ユーザ登録時に送信される登録通知メール |
| パスワード設定の案内 | パスワード設定 URL を案内するメール |
| 通知メールアドレス | ユーザのメールアドレスとは別に通知先を指定する項目 |
