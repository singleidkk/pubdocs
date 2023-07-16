# Google Cloud Directory Sync（GCDS）によるユーザ同期

Google Cloud Directory Sync(GCDS)を使用して、SingleIDのユーザ情報をGoogle Workspaceへ同期する方法を説明します。

!!! info
    [Google Cloud Directory Sync](https://support.google.com/a/topic/2679497?hl=ja&sjid=4615497444537949875-AP){ target=_blank }を参考にしています。

## GCDSのインストール
以下の手順に従いGCDSをインストールします。

[GCDS をダウンロードしてインストールする](https://support.google.com/a/answer/6120989?hl=ja&sjid=4615497444537949875-AP&visit_id=638250880829025489-2610395622&ref_topic=7106512&rd=1){ target=_blank }

## SingleIDのLDAP設定
### LDAPクライアントの登録
GCDSをインストールしたサーバからクラウドLDAPへ接続する際の送信元のグローバルIPをLDAPクライアントとして登録します。LDAPクライアントとして登録していない送信元のIPからは、クラウドLDAPへログインできません。

1. **SingleID 管理者ポータル＞認証＞LDAP＞クライアント**タブ画面を移動します。
2. **登録**ボタンをクリックします。
3. **IPアドレス**には、GCDSをインストールしたサーバからクラウドLDAPへ接続する際の送信元のグローバルIPを入力します。**登録**ボタンをクリックします。

    [![Screenshot](/images/2023-07-16_18-34-34.png)](/images/2023-07-16_18-34-34.png)

### LDAPサービスアカウントの登録
クラウドLDAPへログインするためのサービスアカウントを登録します。初期状態で登録されているユーザ（ldap-bind-user）を利用するか、新規にサービスアカウントを登録します。

1. **SingleID 管理者ポータル＞認証＞LDAP＞サービスアカウント**タブ画面を移動します。
2. **登録**ボタンをクリックします。
3. **ユーザ名**、**パスワード**を入力します。**登録**ボタンをクリックします。

    [![Screenshot](/images/2023-07-16_20-28-40.png)](/images/2023-07-16_20-28-40.png)

## GCDSの設定
**GCDS**をインストールしたサーバにて、**Google Cloud Directory Sync**の**Configuration Manager**から設定します。

### 全般設定
1. サイドメニューの**General Settings**を選択し、**全般設定**画面へ移動します。
2. **User Accounts**のみをチェックします。それ以外のチェックを外してください。

!!! info
    SingleIDのクラウドLDAPは、GCDSのユーザ同期のみをサポートしています。

### Googleドメイン設定
1. サイドメニューの**Google Domain Configuration**を選択し、**Connection Setting**タブ画面へ移動します。
2. 以下の内容で設定し、**Authorize Now**ボタンをクリックして、OAuth経由で認証します。

    | **GCDSの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Primary Domain Name** | Google Workspaceアカウントのドメイン名です。 |

    [![Screenshot](/images/2023-07-16_21-01-23.png)](/images/2023-07-16_21-01-23.png)

### LDAP設定
1. サイドメニューの**LDAP Configuration**を選択し、**LDAP設定**画面へ移動します。
2. 以下の内容で設定し、**Test connection**ボタンをクリックします。

    | **GCDSの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Server Type** | other |
    | **Connection Type** | LDAP+SSL |
    | **Host Name** | **SingleID 管理者ポータル＞認証＞LDAP＞基本情報**タブ画面の**ホスト名（FQDN）**を参照<br> （例：`ldap-02-0001.poc.singleid.jp`）|
    | **Port** | **SingleID 管理者ポータル＞認証＞LDAP＞基本情報**タブ画面の**LDAPSポート番号**を参照<br> （例：`636`） |
    | **Authentication Type** | Simple |
    | **Authorized User** | [LDAPサービスアカウントの登録](#ldapサービスアカウントの登録) で登録したユーザ名 |
    | **Password** | [LDAPサービスアカウントの登録](#ldapサービスアカウントの登録) で登録したパスワード |
    | **Base DN** | **SingleID 管理者ポータル＞認証＞LDAP＞基本情報**タブ画面の**LDAPベースDN**を参照<br> （例：`o=90000013,dc=poc,dc=singleid,dc=jp`） |

    [![Screenshot](/images/2023-07-16_22-08-38.png)](/images/2023-07-16_22-08-38.png)

    !!! info
        Test connectionに失敗する場合には、[証明書に関する問題のトラブルシューティング](https://support.google.com/cloudidentity/answer/9746852?hl=ja){ target=_blank }の**手順 1: .vmoptions ファイルを更新する**をお試しください。

### ユーザアカウント設定
1. サイドメニューの**User Accounts**を選択し、**User Attributes**タブ画面へ移動します。
2. 以下の内容で設定し、**Test connection**ボタンをクリックします。

    | **GCDSの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Email Address Attribute** | mail |
    | **Unique identifier Attribute** | nsuniqueid |
    | **Google Domain User Deletion/Suspension Policy** | Google Workspaceに存在するユーザーの削除や停止についての設定です。必要に応じてオプションのうちの1つを選択します。 |

    [![Screenshot](/images/2023-07-16_21-11-20.png)](/images/2023-07-16_21-11-20.png)

3. **Additional User Attributes**タブ画面へ移動します。
4. 以下の内容で設定します。

    | **GCDSの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Given Name Attribute(s)** | givenName |
    | **Family Name Attribute(s)** | sn |
    | **Display Name Attribute(s)** | cn |

    !!! info
        パスワードの同期はサポートしていません。

    [![Screenshot](/images/2023-07-16_21-29-53.png)](/images/2023-07-16_21-29-53.png)

5. **Search Rules**タブ画面へ移動します。
6. **Add Search Rule**ボタンをクリックします。
7. 以下の内容で設定し、**OK**ボタンをクリックします。

    | **GCDSの設定項目** | **設定内容** |
    | :--- | :--- |
    | **Rule** | (objectClass=person) |
    | **Base DN** | **SingleID 管理者ポータル＞認証＞LDAP＞基本情報**タブ画面の**LDAPユーザDN**を参照<br> （例：`ou=users,o=90000013,dc=poc,dc=singleid,dc=jp`） |

    [![Screenshot](/images/2023-07-16_21-36-42.png)](/images/2023-07-16_21-36-42.png)

### 通知とログ記録
**Notifications**、**Logging**画面へ移動し、必要に応じて、通知とログ記録の設定を行います。

### 同期
1. Sync画面へ移動します。
2. **Validation Results**に、設定の検証結果が表示されます。必要な情報が不足していると、エラーメッセージが表示され、追加する必要が設定項目が示されます。

    [![Screenshot](/images/2023-07-16_21-56-18.png)](/images/2023-07-16_21-56-18.png)

3. すべての必要事項が設定できたら、**Simulate Sync**ボタンをクリックし、同期テストをします。同期テストが完了したら、結果画面を閉じます。

    [![Screenshot](/images/2023-07-16_21-58-06.png)](/images/2023-07-16_21-58-06.png)

4. **Sync & apply changes**をクリックして、クラウドRADIUSとGoogle Workspaceで同期します。同期したユーザは、同期完了後に、Google Workspaceで確認できます。

    [![Screenshot](/images/2023-07-16_22-01-19.png)](/images/2023-07-16_22-01-19.png)
