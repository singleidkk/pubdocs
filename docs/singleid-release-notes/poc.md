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