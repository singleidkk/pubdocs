## 2024-09-18

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.3.0 |
| radius dictionary | 1.5.0 |
| www | **3.7.3** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.17.6** |
)

### 修正

**顧客/MSP管理者ポータル**

* セッションの有効期限が切れた場合に、Not Foundのエラーがブラウザに表示される問題を修正

## 2024-09-10

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.3.0 |
| radius dictionary | 1.5.0 |
| www | **3.7.2** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.17.5** |
)

### 新機能または改善

**MSP管理者ポータル**

* 顧客アカウント一覧画面にある顧客アカウントに解約予定日が設定されている場合、顧客担当者/販売店担当者へ解約予定日の30日前/60日前に契約期限をメール通知します。

## 2024-09-09

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.3.0 |
| radius dictionary | 1.5.0 |
| www | **3.7.1** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.17.2** |
)

### 新機能または改善

**顧客管理者ポータル**

* ユーザ一覧画面で、姓の列に、Firstname、名の列にLastnameが表示されていた問題を修正

## 2024-07-24

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.3.0 |
| radius dictionary | 1.5.0 |
| www | **3.7.0** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.17.0** |
)

### 新機能または改善

**顧客管理者ポータル**

* 登録ユーザが多い場合に、**ユーザ一覧画面の表示**、**RADIUSの簡易サイト登録画面の表示**、**ユーザのCSV/EXCELエクスポート**に時間がかかる問題を改善
* CSVファイルによるユーザの一括登録時に、親子関係のあるグループの指定方法を変更しました。具体的には、**/親グループ/子グループ**のように親子関係を指定する方法から、**子グループ**だけを指定する方法へ変更しました。

## 2024-07-24

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.3.0 |
| radius dictionary | 1.5.0 |
| www | 3.6.13 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.16.2** |
)

### 新機能または改善

**RADIUS**

* 拡張RADIUSサーバを利用してサイト登録する場合、**サイト識別する属性**に、NAS-Identifierを選択し、その**属性値**に、`**MACADDR**`を設定するとMACアドレスの小文字ハイフンなしにマッチするようになる。
* 拡張RADIUSサーバを利用してサイト登録する場合、**サイト識別する属性**に、NAS-Identifierを選択し、その**属性値**に、`**BUFFALO**`を設定するとBuffaloのベンダーコードにマッチするようになる。BuffaloのAirStation Proは、NAS-Identifierに、機器のMACアドレス（小文字ハイフンなし）をRADIUSサーバへ送信しています。

## 2024-07-21

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **10.3.0** |
| radius dictionary | 1.5.0 |
| www | 3.6.13 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.16.0** |
)

### 新機能または改善

**RADIUS**

* 拡張RADIUSサーバを利用してサイト登録する場合、**サイト識別する属性**に、NAS-Identifierを選択し、その**属性値**の文字列で、送信されてきたRADIUSリクエストのNAS-Identifier属性に対して部分一致検索するように変更します。

## 2024-07-14

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 10.2.0 |
| radius dictionary | 1.5.0 |
| www | 3.6.13 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.1 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.14.3** |
)

### 修正
**RADIUS**

* Blast RADIUS攻撃（CVE-2024-3596）の脆弱性を修正

## 2024-06-30

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **10.2.0** |
| radius dictionary | 1.5.0 |
| www | **3.6.13** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | **11.3.1** |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.14.2** |
)

### 新機能または改善
**RADIUS**

* システムへRADIUSの設定が反映されているかどうかを表示

    [![Screenshot](/images/2024-06-24_20-34-03.png)](/images/2024-06-24_20-34-03.png)

### 修正
**アプリ連携**

* アプリに関連付けたユーザ一覧が100件以上表示できない問題を修正

## 2024-06-18

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **10.1.1** |
| radius dictionary | 1.5.0 |
| www | **3.6.12** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.3-1 |
| deploy | **1.13.8** |
)

### 修正
**RADIUS**

* ユーザロックアウト機能の最適化

**顧客管理者ポータル**

* 拡張RADIUSサーバのポートが未登録の場合、標準RADIUSサーバのサイト登録ができない問題を修正

## 2024-05-31

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **10.0.0** |
| radius dictionary | **1.5.0** |
| www | **3.6.9** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | **3.2.3-1** |
| deploy | **1.13.2** |
)

### 新機能または改善
**RADIUS**

* **拡張RADIUSサーバ**を実装しました。今までのRADIUSサーバは**標準RADIUSサーバ**として継続してご利用可能です。それぞれのRADIUSサーバには以下のような違いがあります。

| **比較項目** | **標準RADIUSサーバ** | **拡張RADIUSサーバ** |
| :--- | :--- | :--- |
| **インターネット接続環境とクラウドRADIUSの利用可否**||
| 静的グローバルIPv4アドレス | **利用可能** | **利用可能** |
| 動的グローバルIPv4アドレス | **利用可能（DDNSの利用）** | **利用可能** |
| 静的グローバルIPv6アドレス | **利用可能** | **利用可能（IPv4 over IPv6の利用）** |
| 動的グローバルIPv6アドレス | **利用可能（DDNSの利用）** | **利用可能（IPv4 over IPv6の利用）** |
| 共用グローバルIPv4アドレス | 利用不可 | **利用可能** |
| **機能** ||
| RadSec | 利用不可 | **利用可能** |
| **ポリシー** ||
| RADIUSクライアントの識別方法 | RADIUSクライアントの**グローバルIPアドレス** | 送信される**NAS-IP-Address属性**または**NAS-Identifier属性**の属性値 |

!!! info
    * **拡張RADIUSサーバを利用することで、顧客のインターネット接続で割り当てられるIPアドレスを気にせずに、クラウドRADIUSをご利用いただけます。**その際、RADIUSを利用するネットワーク機器が送信するNAS-IP-Address属性またはNAS-Identifier属性をクラウドRADIUSへ設定することが必要です。
    * 標準RADIUSサーバと拡張RADIUSサーバは併用することができます。

!!! warning
    標準RADIUSサーバを利用するサイトと拡張RADIUSサーバを利用するサイトは、**合計10サイトまでに登録が制限**されています。

### 修正
**RADIUS**

* クライアントのシークレットを変更後、しばらく接続ができない問題を修正

## 2024-05-17

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **9.1.0** |
| radius dictionary | 1.4.0 |
| www | 3.6.6 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.12.0** |
)

### 新機能または改善
**RADIUS**

* 丸紅情報システムズ WDSユーザ認証の簡易設定で、一定期間内でログイン試行に5回失敗した場合にアカウントを無効にする機能をサポート

## 2024-05-08

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.6.0 |
| radius dictionary | 1.4.0 |
| www | 3.6.6 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.10.2** |
)

### 新機能または改善
**顧客管理者ポータル/MSP管理者ポータル**

* ポータルへのログイン失敗時のユーザのロックアウト機能をサポート

## 2024-04-22

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.6.0 |
| radius dictionary | 1.4.0 |
| www | 3.6.6 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.4.0 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.10.2** |
)

### 新機能または改善
**RADIUS**

* ユーザ名に@（アットマーク）を含み、@よりも後に.（ドット）がない場合に、RADIUS認証を拒否しないように仕様変更

## 2024-04-19

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.6.0** |
| radius dictionary | **1.4.0** |
| www | **3.6.6** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | **7.4.0** |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.10.1** |
)

### 新機能または改善
**RADIUS**

* **認証＞RADIUS＞簡易設定＞登録/編集画面＞基本情報**タブにある**ワンタイムパスワード強制**の設定項目をなくして、**VPNアクセスの認証**、**管理者アクセスの認証**、**その他のアクセス**の認証タブに２要素認証（ワンタイムパスワード）のみ許可する設定を追加したことで、アクセスの種類に応じて、２要素認証を強制することが可能

    [![Screenshot](/images/2024-04-20_9-33-29.png)](/images/2024-04-20_9-33-29.png)

* YAMAHA RTXの簡易設定画面から無線アクセスの認証タブを削除

**ネットワーク機器連携カタログ**

* 丸紅情報システムズ WDSユーザ認証/デバイス認証の追加

## 2024-04-16

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.3.1 |
| www | **3.6.5** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.3.1 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.9.0** |
)

### 修正
**ユーザ**

* ユーザの一括登録およびエクスポート時の子グループの取り扱いを修正

## 2024-04-03

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.3.1 |
| www | 3.6.4 |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | **7.3.1** |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.8.17** |
)

### 新機能または改善
**RADIUS**

* Calling-Station-Id属性の値に、文字列以外が含まれていた場合にそれらを除外

**ネットワーク機器連携カタログ**

* PIOLINK TiFRONT-APの追加
* ワイズ STEALTHONE APの追加

## 2024-03-27

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.3.1 |
| www | **3.6.4** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.2.4 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.8.14** |
)

### 新機能または改善
**RADIUS**

* FortiGateのカタログでのみでサポートしていた**証明書認証（EAP-TLS）のみ許可**する機能を全てのネットワーク機器でサポート（**無線アクセスの認証**タブと**ネットワークアクセスの認証**タブに、**証明書認証（EAP-TLS）のみ許可**の設定項目を追加）

    [![Screenshot](/images/2023-03-11_19-43-11.png)](/images/2023-03-11_19-43-11.png)

## 2024-03-12

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | **1.3.1** |
| www | **3.6.1** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | **7.2.4** |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.8.11** |
)

### 新機能または改善
**RADIUS**

* H3Cのベンダー固有アトリビュート（VSA）をサポート

**アプリ連携**

* DropboxのSSOをサポート
* Sustainsys.SAML2ライブラリのSSOをサポート
* Spring SecurityフレームワークのSSOをサポート

### 修正
**MSP管理者ポータルのUI**

* **MSP管理者ポータル＞管理アカウント＞顧客アカウント一覧**のエクスポート機能で、エクスポートされるファイルの契約日項目の不要な文字列を削除

## 2024-03-05

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.2.0 |
| www | **3.6.0** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.2.1 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.8.7** |
)

### 新機能または改善
**MSP管理者ポータルのUI**

* **MSP管理者ポータル＞管理アカウント＞顧客アカウント一覧**で表示されている**顧客アカウント**を編集し、**画面のカスタマイズ**タブ画面で、顧客管理者ポータルのメニューの表示/非表示の設定が可能

[![Screenshot](/images/2024-03-05_9-20-51.png)](/images/2024-03-05_9-20-51.png)

## 2024-01-31

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.2.0 |
| www | **3.5.27** |
| pki | 7.2.1 |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.2.1 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.8.0** |
)

### 修正
**MSP管理者ポータルのUI**

* 契約管理者権限を持った管理者が、MSP管理者ポータルの編集画面を開いたままブラウザのウィンドウを閉じると、ポータルへ再度アクセスしたときに、画面が正常に表示されない問題を修正。

## 2023-12-23

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.2.0 |
| www | **3.5.26** |
| pki | **7.2.1** |
| report | 2.3.1 |
| provisioning | 11.3.0 |
| provisioning appdata | 7.2.1 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | 1.7.3 |
)

### 新機能または改善
**PKI**

* 証明書の自動インストールバッチを一般ユーザ権限で実行するように改善

**クラウドRADIUS**

* FortiGateのVPNの認証時に、FortiGateのグループ情報を応答できるように改善

## 2023-12-14

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.5.1 |
| radius dictionary | 1.2.0 |
| www | **3.5.24** |
| pki | 7.2.0 |
| report | 2.3.1 |
| provisioning | **11.3.0** |
| provisioning appdata | 7.2.1 |
| tun | 3.3.1 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.7.0** |
)

### 新機能または改善
**顧客管理者ポータル**

* 顧客管理者ポータルの顧客切り替え機能に、顧客アカウント番号または会社名で検索する機能を追加

**ユーザ**

* ユーザ一覧画面の一括登録で、誤ったデータを取り込まないように改善

!!! warning
    ユーザ一括登録で取り込むCSVフォーマットが変更されています。ご注意ください。

### 修正
**UI**

* カタログに表示する製品、サービスがない場合、表示レイアウトの修正

## 2023-11-16

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.5.1** |
| radius dictionary | 1.2.0 |
| www | **3.5.18** |
| pki | **7.2.0** |
| report | **2.3.1** |
| provisioning | **11.2.1** |
| provisioning appdata | 7.2.1 |
| tun | **3.3.1** |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
| deploy | **1.5.6** |
)

### 新機能または改善
**API**

* APIに対するヘルスチェックの改善

### 修正
**UI**

* RADIUS簡易設定のネットワークスイッチのテンプレートを修正

## 2023-11-07

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.3.0 |
| radius dictionary | 1.2.0 |
| www | **3.5.17** |
| pki | **7.0.1** |
| report | 2.1.0 |
| provisioning | **11.0.1** |
| provisioning appdata | **7.2.1** |
| tun | 3.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**アプリ連携**

* NI Collabo 360のSSOをサポート

**ログ**

* **ログ＞SSO認証ログ**に、SSOのログイン情報の履歴を出力

**管理**

* **管理＞詳細設定**画面から、**ログ＞SSO認証ログ**画面の履歴の消去が可能

**証明書**

* サブジェクトの代替名としてIPを指定したサーバ証明書を発行できるように改善
[![Screenshot](/images/2023-11-03_15-28-06.png)](/images/2023-11-03_15-28-06.png)

### 修正
**LDAP**

* 認証＞LDAP＞基本情報タブの内容を変更

**証明書**

* 認証＞証明書＞証明書タブで証明書のダウンロードの挙動を修正

**RADIUS**

* 簡易設定画面からクライアントIPの登録ができない場合があることを修正

## 2023-10-21

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.3.0 |
| radius dictionary | 1.2.0 |
| www | **3.5.8** |
| pki | 6.1.0 |
| report | 2.1.0 |
| provisioning | 11.0.0 |
| provisioning appdata | 7.1.0 |
| tun | 3.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**ユーザ**

* パスワードで使用できる特殊文字にチルダ(~)を追加

**管理者**

* パスワードで使用できる特殊文字にチルダ(~)を追加

## 2023-10-04

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.3.0** |
| radius dictionary | 1.2.0 |
| www | **3.5.7** |
| pki | **6.1.0** |
| report | 2.1.0 |
| provisioning | 11.0.0 |
| provisioning appdata | 7.1.0 |
| tun | **3.0.0** |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**ユーザ**

* ユーザ一覧画面からユーザを登録したときに送信されるユーザ登録通知メールについて、指定したメールアドレス宛に送信可能
* ユーザ一覧画面からユーザのクライアント証明書を発行したときの送信される証明書発行通知メールについて、指定したメールアドレス宛に送信可能
[![Screenshot](/images/2023-10-04_10-24-50.png)](/images/2023-10-04_10-24-50.png)

!!! warning
    上記２つをサポートしたことで、ユーザの一括登録に使用するCSVファイルのフォーマットが変更されています。ご注意ください。

**管理**

* **管理＞詳細設定**画面から、**ログ＞操作ログ**画面の履歴の消去が可能
* **管理＞詳細設定**画面から、**ログ＞RADIUS認証ログ**画面の履歴の消去が可能
* **管理＞詳細設定**画面から、**認証＞証明書＞証明書タブ**画面の履歴の消去が可能
[![Screenshot](/images/2023-10-04_10-28-12.png)](/images/2023-10-04_10-28-12.png)

!!! warning
    上記３つについて、消去した履歴は復旧できません。ご注意ください。

**DDNSサービス**

* **管理＞ネットワーク**画面から登録できるDDNSサービスで、IPv6のDDNSホストの登録をサポート
[![Screenshot](/images/2023-10-04_10-30-33.png)](/images/2023-10-04_10-30-33.png)

**RADIUS**

* IPv6サポート（プレビュー機能）
* RADSEC（RADIUS over TLS）サポート（プレビュー機能）

!!! warning
    プレビュー機能を評価したい場合には、SingleIDまでお問い合わせください。

### 修正
**UI**

* サイドメニューにあったお支払いメニューを削除

**ユーザ**

* ユーザ一覧画面で、100ユーザ以上登録した場合に、2ページ目の画面で、ユーザを選択して選択実行ボタンがクリックできない問題を修正
* ユーザの一括登録機能で、登録した場合に、契約数以上のユーザが登録できてしまう問題を修正
* **管理＞詳細設定**画面で、**メールアドレスの重複を許可**を有効にしたとき、メールアドレスが重複しているユーザを一括登録できない問題を修正

## 2023-09-14

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.2.0** |
| radius dictionary | 1.2.0 |
| www | 3.5.1 |
| pki | 6.0.0 |
| report | 2.1.0 |
| provisioning | 11.0.0 |
| provisioning appdata | 7.1.0 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**RADIUS**

* サイトの有効化/無効化の切り替えが設定に反映されない場合があることを修正

## 2023-08-18

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | 3.5.1 |
| pki | 6.0.0 |
| report | **2.1.0** |
| provisioning | 11.0.0 |
| provisioning appdata | **7.1.0** |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**セキュリティ診断**

* SingleID Health for AWSをカタログへ掲載
* SingleID Health for Azureをカタログへ掲載

## 2023-08-17

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.5.1** |
| pki | **6.0.0** |
| report | 2.0.2 |
| provisioning | 11.0.0 |
| provisioning appdata | 7.0.0 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**PKI**

* 認証＞証明書＞証明書タブ画面で、サーバ証明書が発行可能（VPNゲートウェイ機器によっては、クライアント証明書認証をする場合に、同じCA局で発行したサーバ証明書をインストールする必要があるため）
[![Screenshot](/images/2023-08-17_13-19-31.png)](/images/2023-08-17_13-19-31.png)

## 2023-08-04

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.4.2** |
| pki | **5.0.2** |
| report | 2.0.2 |
| provisioning | 11.0.0 |
| provisioning appdata | 7.0.0 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**PKI**

* セットアップ時の混乱を避けるために、証明書のWindows自動構成ファイルのフォルダ構造を変更
* 右クリックしてから「管理者として実行」を選択した場合に、setup.batが実行できるように証明書のWindows自動構成ファイルを改善

**メール通知**

* パスワードの更新およびパスワードのリセット時のメール通知の宛名をユーザ名表記へ統一
* パスワードの更新およびパスワードのリセット時のメール通知のアクション用リンクの表記を改善
* 証明書の自動構成ファイルの通知メールの文面を改善

**管理者ポータル**

* 低解像度のディスプレイでは、左サイドメニューの全てが表示されないことを改善（ディスプレイ解像度の高さが720以上にしてください。）

## 2023-08-02

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.4.1** |
| pki | **5.0.0** |
| report | **2.0.2** |
| provisioning | **11.0.0** |
| provisioning appdata | **7.0.0** |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**セキュリティ診断**

* SingleID Health for M365をカタログへ掲載

**MSP管理者ポータル**

* ポイント履歴画面のスポット対応の消費ポイント

## 2023-07-21

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | 3.3.0 |
| pki | 4.0.8 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | **5.8.2** |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**アプリ連携**

* Google Workspaceのサポート

## 2023-07-12

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.3.0** |
| pki | 4.0.8 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**アプリ連携**

* SAMLのクライアントIDが固定ではないアプリを登録できない問題を修正

## 2023-07-02

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.2.9** |
| pki | 4.0.8 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**ユーザ**

* ユーザの一括登録で、CSVファイル内でグループ指定していない場合、意図しないグループへ登録されてしまう問題を修正

## 2023-06-26

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | **3.2.6** |
| pki | 4.0.8 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**管理者ポータル**

* 設定画面（モーダル表示）の保存時にまれに設定保存されない問題を修正

## 2023-06-21

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.3 |
| radius dictionary | 1.2.0 |
| www | 3.2.5 |
| pki | **4.0.8** |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**PKI**

* プロビジョニング時の中間CAのメタデータの問題の修正

## 2023-06-14

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.1.3** |
| radius dictionary | **1.2.0** |
| www | 3.2.5 |
| pki | 4.0.6 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**RADIUS**

* 内部ログ出力の改善
* 辞書ファイルの更新

## 2023-06-12

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.2 |
| radius dictionary | 1.1.0 |
| www | **3.2.5** |
| pki | 4.0.6 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**MSP管理者ポータル**

* 承認待ちアカウント一覧に表示されているアカウントを削除/編集

## 2023-06-06

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.2 |
| radius dictionary | 1.1.0 |
| www | **3.2.2** |
| pki | 4.0.6 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | 2.0.0 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**RADIUS**

* 認証＞RADIUS＞簡易設定のYAMAHA WLXの設定時、クライアント重複のエラーメッセージが正しく表示されない問題を修正

## 2023-05-31

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.1.2** |
| radius dictionary | 1.1.0 |
| www | **3.2.1** |
| pki | 4.0.6 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| tun | **2.0.0** |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**RADIUS**

* 同じサーバ番号内で、RADIUSクライアントIPが重複した場合のエラー表示と関連する問題を修正

**ユーザ**
* ユーザおよび管理者のパスワードで使用できる特殊文字の変更 #169

    パスワードに利用可能な特殊文字 `!#%$?&'():;"+*<>_-@^,.`

## 2023-05-26

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.1.1** |
| radius dictionary | 1.1.0 |
| www | 3.1.7 |
| pki | **4.0.6** |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**PKI**

* Windowsの証明書の自動構成ツールの文字コードの問題を修正

**RADIUS**

* 指定先のサーバ内に同じRADIUSクライアントIPが存在した場合のエラー処理

## 2023-05-16

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.0 |
| radius dictionary | 1.1.0 |
| www | **3.1.7** |
| pki | 4.0.5 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**DDNS**

* 同一のDDNSホストの登録時のエラー処理 #134
  
**MSP管理者ポータル**

* サポートアカウントの管理者でログインしたときに、ある条件下で、MSP管理者ポータルで顧客アカウントが変更可能となる問題を修正 #133

### 新機能または改善
**MSP管理者ポータル**

* 顧客アカウント一覧画面の契約日を特権MSPおよびMSP管理者は変更可能 #174
  
## 2023-05-12

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.0 |
| radius dictionary | 1.1.0 |
| www | 3.1.5 |
| pki | **4.0.5** |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**PKI**

* PKIから削除された証明書をAPIでダウンロードしようとしたときの処理の修正 S/DP#5

### 新機能または改善
**PKI**

* Windowsの証明書の自動構成ファイルの改善（ログ出力） S/DP#4
  
**IDP**

* TLSv1.3の有効化
* DHの1024ビットの鍵長を無効化

## 2023-05-07

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.1.0 |
| radius dictionary | 1.1.0 |
| www | 3.1.5 |
| pki | **4.0.3** |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**PKI**

* ダウンロードリンクの有効期限が正しくない問題を修正 S/DP#1

## 2023-05-07

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.1.0** |
| radius dictionary | 1.1.0 |
| www | 3.1.5 |
| pki | 4.0.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**ログ**

* RADIUSのログを定期的に削除 S/DF#9

## 2023-04-26

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.0.0 |
| radius dictionary | 1.1.0 |
| www | **3.1.5** |
| pki | 4.0.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**顧客管理者ポータル**

* 管理者の操作をログに管理者が正しく表示されていない #140

**MSP管理者ポータル**

* サポートアカウントで顧客アカウントの登録が可能となっている #133

## 2023-04-26

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.0.0 |
| radius dictionary | 1.1.0 |
| www | **3.1.4** |
| pki | 4.0.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**顧客管理者ポータル**

* ユーザ登録時に、パスワードポリシーがマッチしないと氏名などの情報が空となる #149

## 2023-04-22

[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.0.0 |
| radius dictionary | 1.1.0 |
| www | **3.1.3** |
| pki | **4.0.0** |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善

**MSP管理者ポータル**

* サポートアカウント機能 #133

    販売店様がSingleIDをサポートする目的で使用するアカウントです。特権MSPアカウント、MSPアカウントとの違いについては、別途、MSP管理者ガイド（作成中）をご確認ください。

**PKI**

* 証明書インストールの自動構成ツールをユーザへメールで配布 #153
  
    自動構成ツールを実行すると、インストールおよび証明書一覧のメモ（**顧客管理者ポータル＞認証＞証明書＞証明書タブ**画面）に、**ユーザ名**（ドメイン\ユーザ形式）、**コンピュータ名**の情報を記録します。

    **顧客管理者ポータル＞ユーザ一覧**画面または**顧客管理者ポータル＞認証＞証明書＞証明書タブ**画面から配布可能です。
    [![Screenshot](/images/2023-04-22_9-28-32.png)](/images/2023-04-22_9-28-32.png)

    **顧客管理者ポータル＞認証＞証明書＞証明書タブ**画面で、証明書のインストール日時、証明書がインストールされたWindows PCの情報が確認できます。
    [![Screenshot](/images/2023-04-22_9-34-39.png)](/images/2023-04-22_9-34-39.png)

    !!! info
        **顧客管理者ポータル＞管理＞詳細設定**画面にて、証明書の**発行時にメール通知**が有効となっていない場合には、メールで配布できません。 

**ネットワークサービス**

* トンネルおよびDDNSサービス機能 #134

    **顧客管理者ポータル＞管理＞ネットワーク**画面からトンネルおよびダイナミックDNSを利用するためのAPIキーを発行できます。

    [![Screenshot](/images/2023-04-22_9-43-21.png)](/images/2023-04-22_9-43-21.png)

    !!! info
        トンネルサービスは、SingleID Boxでのみ利用可能です。ダイナミックDNSサービスは、シェルスクリプトやバッチファイルでAPIを実行することで利用できます。また、FortigateのWebhook機能でAPIを実行することもできます。

**ユーザ**

* ユーザ登録時のメールアドレスの項目を他ユーザとの重複を許可 #161

    複数ユーザに同一のメールアドレスを設定することできます。デフォルトは無効です。

    [![Screenshot](/images/2023-04-22_9-57-19.png)](/images/2023-04-22_9-57-19.png)

    !!! warning
        **メールアドレスが重複することにより、以下の制限があります。**
        
        **利用不可**

        * メールアドレスではユーザポータルへログインできない。
        * クライアント証明書認証では、管理者ポータルへログインできない。

        **影響**

        * セルフパスワードリセット機能のメール通知
        * 証明書のダウンロードリンクのメール通知
        * SSO（クラウドサービスはメールアドレスを用いて認証することが多い）
        * 証明書を用いたメールの暗号化 （デフォルトで作成される証明書には、メールアドレス項目をSANとして設定）

* **顧客管理者ポータル＞ユーザ一覧**画面のユーザ登録/編集画面で、姓名の英字の欄を必須入力から任意入力にする #167

    **顧客管理者ポータル＞ユーザ一覧画**面のユーザ登録/編集画面の**姓名の英字**もしくは**姓名の漢字**のどちらかを必須入力とします。

* ユーザおよび管理者のパスワードで使用できる特殊文字の変更 #169

    パスワードに利用可能な特殊文字 `!#%$?&'():;"+*<>_@^,.`

### 修正

**MSP管理者ポータル**

* MSP管理者ポータルで特権MSPやMSPが自身のアカウントのポイントを変更できてしまう #164

## 2023-04-18
[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 8.0.0 |
| radius dictionary | 1.1.0 |
| www | 3.0.4 |
| pki | 3.1.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善
**通知メール**

* アカウントの更新およびパスワードリセットのメール通知内容の変更

## 2023-04-09
[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | **8.0.0** |
| radius dictionary | 1.1.0 |
| www | 3.0.4 |
| pki | 3.1.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**RADIUS**

* 設定情報のバックアップデータのパージの問題

## 2023-03-30
[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 7.0.1 |
| radius dictionary | 1.1.0 |
| www | **3.0.4** |
| pki | 3.1.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 修正
**ログ**

* 操作の集計通知で送信される集計日がメール送信の2日前になっていた問題
* MSP管理者による操作ログ閲覧の問題

## 2023-03-10
[](
| **コンポーネント** | **バージョン** |
| :-- | :-- |
| radius | 7.0.1 |
| radius dictionary | 1.1.0 |
| www | 3.0.3 |
| pki | 3.1.0 |
| report | 1.2.2 |
| provisioning | 9.0.0 |
| provisioning appdata | 5.7.5 |
| auth module | 16.1.1 |
| ldap module | 2.0.15-1 |
| log module | 4.4.1-1 |
| radius module | 3.2.1-1 |
)

### 新機能または改善

**ログ**

* ログ＞操作ログ画面で、管理者の操作内容をログへ出力

    [![Screenshot](/images/2023-03-11_19-35-36.png)](/images/2023-03-11_19-35-36.png)

* 管理＞詳細設定画面で、操作ログの集計を有効にすることで、管理者および管理＞詳細設定画面の通知メールアドレスあてに操作の集計通知を１日１回送信

    [![Screenshot](/images/2023-03-11_19-39-42.png)](/images/2023-03-11_19-39-42.png)

**ユーザ**

* ユーザ登録時に、ユーザの登録通知をユーザのメールアドレス宛に送信
* ユーザ情報をCSVまたはEXCEL形式でエクスポート

    [![Screenshot](/images/2023-03-11_19-41-19.png)](/images/2023-03-11_19-41-19.png)

* ユーザ情報のCSVインポート機能のインポートするCSV形式を変更

**RADIUS**

* FortiGateの簡易設定で、FortiAPをサポートするために、無線アクセスの認証タブを追加。
* FortiGateの簡易設定で、無線アクセスの認証を証明書認証のみに制限可能

    [![Screenshot](/images/2023-03-11_19-43-11.png)](/images/2023-03-11_19-43-11.png)

**証明書**

- 証明書発行時の通知メールの内容を改善

**管理者**

- 管理者登録時に、管理者の登録通知を管理者のメールアドレス宛に送信

**MSP管理者ポータル**

* MSP管理者ポータル＞管理アカウント＞顧客アカウント一覧画面で、顧客アカウント登録時に、販売店情報の入力項目を追加

    [![Screenshot](/images/2023-03-11_19-46-10.png)](/images/2023-03-11_19-46-10.png)

* MSP管理者ポータル＞管理アカウント＞顧客アカウント一覧画面で、顧客アカウントを選択し、選択実行＞納品通知を選択することで、顧客担当者および販売店担当者のメールアドレスへ納品通知メールを送信
* MSP管理者ポータル＞管理アカウント＞顧客アカウント一覧画面で、顧客アカウントの情報をCSVまたはEXCEL形式でエクスポート

    [![Screenshot](/images/2023-03-11_19-44-50.png)](/images/2023-03-11_19-44-50.png)
