# Microsoft 365（M365）連携

設定を始める前に、[一般的な考慮事項](considerations.md)をお読みください。

#### M365を新規導入する場合

[事前作業のM365のカスタムドメイン設定](#m365のカスタムドメイン設定)から始めてください。

#### M365をすでに運用中の場合

設定作業を始める前に、**以下の全ての項目に該当することを確認**してください。該当しない場合には、シングルサインオンおよびプロビジョニングを行うことができません。全てに該当している場合には、[シングルサインオンおよびプロビジョニング設定](#シングルサインオンおよびプロビジョニング設定)から始めてください。

* シングルサインオンするUPN（ユーザプリンシパル名）のドメインをカスタムドメインとして追加済みであること
* シングルサインオンするカスタムドメインは、既定のドメインに設定されていないこと
* シングルサインオンするカスタムドメインに、フェデレーション（SAML）の設定がされていないこと
* 運用開始後、シングルサインオンするカスタムドメインで、Azure AD ConnectやIDプロバイダーとの同期設定をしたことがないこと
* SingleIDユーザの組織のメールアドレスとUPNが同じであること
* M365の全体管理者はシングルサインオンするカスタムドメインとは異なるドメインに配置されていること

## 事前作業
### M365のカスタムドメイン設定
SingleIDのユーザの組織のメールアドレスのメールドメインをM365のカスタムドメインとして追加します。M365の契約時に提供される**xxxxx.onmicrosoft.com**ドメインは**既定**のドメインとして設定し、シングルサインオンの対象となるドメインとして利用できません。

#### カスタムドメインの追加方法
1. [Microsoft 365 の管理センター](https://admin.microsoft.com/){target=_blank}に全体管理者でログインします。
2. **設定＞ドメイン**画面へ移動します。
3. **ドメインの追加**ボタンをクリックします。
4. 追加するドメインの名前を入力し、**このドメインを使用する**ボタンをクリックします。
5. ドメインの所有を確認する方法を選択します。
6. Microsoft がドメインを使用するために必要なDNS変更を行います。
7. **完了**ボタンをクリックします。

詳細は、[Microsoft 365 のヘルプ記事](https://learn.microsoft.com/ja-jp/microsoft-365/admin/setup/add-domain?view=o365-worldwide){target=_blank}を参照してください。

#### xxxxx.onmicrosoft.comドメインを既定のドメインにする方法
1. [Microsoft 365 の管理センター](https://admin.microsoft.com/){target=_blank}に全体管理者でログインします。
2. **設定＞ドメイン**画面へ移動します。
3. **ドメイン**画面で、xxxxx.onmicrosoft.comドメインを選択します。
4. **既定に設定**ボタンをクリックします。

詳細は、[Microsoft 365 のヘルプ記事](https://learn.microsoft.com/ja-jp/microsoft-365/admin/setup/domains-faq?view=o365-worldwide#microsoft-365-------------------------------){target=_blank}を参照してください。

!!! info
    Exchange Onlineが生成する組織外向けの配信不能通知（NDR）の送信元メールアドレスは、**postmaster@組織の既定のドメイン**です。そのため、NDRの送信元は、**postmaster@xxxxx.onmicrosoft.com**となります。組織外向けのNDRの送信元メールアドレスを変更したい場合には、Windows PowerShell `Set-TransportConfig`コマンドを使用します。詳細は、[Exchange Onlineの記事](https://learn.microsoft.com/ja-jp/exchange/mail-flow-best-practices/configure-external-postmaster-address){target=_blank}を参照してください。
    
## シングルサインオンおよびプロビジョニング設定
### M365のアプリ登録
1. [Azureポータル](https://portal.azure.com){target=_blank}へ全体管理者でログインします。
2. **Azure Active Directory**管理画面に移動します。（画面上部の検索ボックスに**Active**と入力し、表示された**Azure Active Directory**をクリックします。）

    [![Screenshot](/images/2022-09-26_7-46-39.png)](/images/2022-09-26_7-46-39.png)

3. サイドメニューから**アプリの登録**をクリックします。

    [![Screenshot](/images/2022-09-26_7-50-38.png)](/images/2022-09-26_7-50-38.png)

4. **新規登録**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_7-53-45.png)](/images/2022-09-26_7-53-45.png)

5. 以下の内容で設定し、**登録**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意です。（例：SingleID） |
    | **サポートされているアカウントの種類** | この組織ディレクトリのみに含まれるアカウント (シングル テナント) |

    [![Screenshot](/images/2022-09-26_7-55-21.png)](/images/2022-09-26_7-55-21.png)

6. **アプリケーション (クライアント) ID**、**ディレクトリ (テナント) ID**の値をコピーして、保存しておきます。

    [![Screenshot](/images/2022-09-26_8-53-32.png)](/images/2022-09-26_8-53-32.png)

7. サイドメニューから**証明書とシークレット**をクリックします。

    [![Screenshot](/images/2022-09-26_8-15-17.png)](/images/2022-09-26_8-15-17.png)

8. **クライアント シークレット**タブへ移動し、**新しいクライアント シークレット**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_8-18-49.png)](/images/2022-09-26_8-18-49.png)

9.  以下の内容で設定し、**追加**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **説明** | 任意です。（例：SingleID連携） |
    | **有効期限** | 24か月 |

    [![Screenshot](/images/2022-09-26_8-28-54.png)](/images/2022-09-26_8-28-54.png)

10. 作成された**クライアント シークレット**は、SingleIDのアプリ登録のプロビジョニング設定で必要となります。**値をコピーして、保存**しておきます。

    [![Screenshot](/images/2022-09-26_8-33-11.png)](/images/2022-09-26_8-33-11.png)

    !!! warning
        クライアント シークレットの値は、画面を遷移すると参照できなくなります。値をコピーし忘れた場合には、作成したクライアント シークレットを削除し、再作成してください。

11. サイドメニューから**API のアクセス許可**をクリックします。

    [![Screenshot](/images/2022-09-26_9-43-40.png)](/images/2022-09-26_9-43-40.png)

12. **アクセス許可の追加**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_9-45-35.png)](/images/2022-09-26_9-45-35.png)

13. **Microsoft API**の**Microsoft Graph**を選択します。

    [![Screenshot](/images/2022-09-26_9-48-40.png)](/images/2022-09-26_9-48-40.png)

14. **アプリケーションの許可**を選択します。

    [![Screenshot](/images/2022-09-26_9-52-01.png)](/images/2022-09-26_9-52-01.png)

15. 以下のアクセス許可を全て追加します。すべて選択したら**アクセス許可の追加**ボタンをクリックします。

    * Directory.ReadWrite.All
    * Group.ReadWrite.All
    * GroupMember.ReadWrite.All
    * Organization.ReadWrite.All
    * User.ReadWrite.All

16. 追加したアクセス許可が一覧に表示されたことを確認し、**管理者の同意を与えます**ボタンをクリックします。

    [![Screenshot](/images/2022-09-28_15-40-18.png)](/images/2022-09-28_15-40-18.png)

17. **はい**をクリックします。

    [![Screenshot](/images/2022-09-26_11-52-42.png)](/images/2022-09-26_11-52-42.png)

### SingleIDのアプリ連携設定
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Microsoft 365の**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-09-26_3-02-06.png)](/images/2022-09-26_3-02-06.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-09-26_3-11-42.png)](/images/2022-09-26_3-11-42.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、M365の設定を行うときに必要となります。
8. **プロビジョニング**タブに移動します。
    
    [![Screenshot](/images/2022-09-26_3-12-56.png)](/images/2022-09-26_3-12-56.png)

9. **プロビジョニング 有効/無効**を**有効**に設定します。
10. **ユーザ削除時の同期の動作 無効/削除**を設定します。**無効**を推奨します。

    * **無効**の場合、SingleIDのM365アプリからユーザを削除した際、M365ではユーザはライセンスが適用されたまま無効化されます。必要に応じて、M365管理センターから、ユーザのライセンス解除またはユーザ削除を手動で行ってください。
    * **削除**の場合、SingleIDのM365アプリからユーザを削除した際、M365のユーザも削除されます。

    !!! danger
        M365のユーザが削除されるとユーザに関連付くデータも削除されます。[元従業員を削除し、データをセキュリティで保護する](https://learn.microsoft.com/ja-jp/microsoft-365/admin/add-users/remove-former-employee?view=o365-worldwide){target=_blank}を参照し、適切にデータを保護してください。

11. 以下の内容で設定し、**登録**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ディレクトリ (テナント) ID** | [M365のアプリ登録](#m365のアプリ登録)の手順6参照 |
    | **アプリケーション (クライアント) ID** | [M365のアプリ登録](#m365のアプリ登録)の手順6参照 |
    | **クライアント シークレット** | [M365のアプリ登録](#m365のアプリ登録)の手順10参照 |
    | **ライセンスの自動割当** | 有効（詳細は、[自動割当可能なライセンス](microsoft_365_autoassign.md)を参照） | 

    [![Screenshot](/images/2022-09-26_3-15-14.png)](/images/2022-09-26_3-15-14.png)

### SingleIDのアプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある*cd ..*チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

### 動作確認（ユーザ同期）

### M365のSAML認証設定
1. 全体管理者として Azure AD ディレクトリにログインします。

    ``` powershell title="PowerShell"
    Connect-MsolService
    ```

2. シングルサインオンするカスタムドメインにSAMLの設定を行います。下表のように、環境に合わせて、PowerShellの変数を変更してから、実行してください。

    | PowerShellの変数 | 変数の値 |
    | :--- | :--- |
    | $domain | シングルサインオンするカスタムドメイン |
    | $AppName | [SingleIDのアプリ連携設定](#singleidのアプリ連携設定)の手順5で設定した名前 |
    | $IdpIssuer | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<md:EntityDescriptor entityID=`　から始まる行のURLを入力します。<br><br>（例：`https://auth-02-0001.poc.singleid.jp/auth/realms/90000013`） |
    | $MySigningCert | SingleIDのIdPエンドポイントメタデータのファイルを開きます。<br>`<ds:X509Certificate>`タグ内のMIIで始まる文字列（例：`MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDw……..`）をコピーして、入力します。 |

    ``` powershell title="PowerShellのサンプルスクリプト"
    $domain = "xxxxx.singleid.jp" 
    $AppName = "m365"
    $IdpIssuer = "https://auth-02-0001.dev.singleid.jp/auth/realms/90000013" 
    $IdpSigningCert = "MIICnzCCAYcCBgF/zubcKTANBgkqhkiG9w0BAQsFADATMREwDw…….." 
    $BrandName = "SingleID"
    $IdpEndpoint = $IdpIssuer + "/protocol/saml"
    $LogOffUrl = "https://portal.office.com" 
    $Protocol = "SAMLP" 
    Set-MsolDomainAuthentication `
        -DomainName $domain `
        -FederationBrandName $BrandName `
        -Authentication Federated `
        -PassiveLogOnUri $IdpEndpoint `
        -ActiveLogOnUri $IdpEndpoint `
        -SigningCertificate $IdpSigningCert `
        -IssuerUri $IdpIssuer `
        -LogOffUri $LogOffUrl `
        -PreferredAuthenticationProtocol $Protocol
    ```

3. SAMLの設定が行えたことを確認します。シングルサインオンするカスタムドメイン**Verified Federated**となっていればSMAL設定がされています。

    ``` powershell title="PowerShell"
    Get-MsolDomain 
    ```

    ``` powershell title="レスポンス"
    Name                            Status   Authentication
    ----                            ------   --------------
    xxxxx.onmicrosoft.com         Verified Managed
    xxxxx.singleid.jp             Verified Federated
    ```

### 動作確認（シングルサインオン）
1. https://portal.office.com へアクセスします。
2. ユーザ同期されM365に登録されたユーザを入力して、**次へ**をクリックします。

    [![Screenshot](/images/2022-09-26_18-57-56.png)](/images/2022-09-26_18-57-56.png)

#### M365のログイン画面で、パスワードの入力を要求された場合

[![Screenshot](/images/2022-09-26_19-00-03.png)](/images/2022-09-26_19-00-03.png)

Microsoftのドキュメントによると、SAML設定が有効となるまで、2時間ほどかかる場合があります。しばらくたってからお試しください。

#### M365のログイン画面で、アカウント保護にご協力くださいの画面が表示された場合

[![Screenshot](/images/2022-09-26_17-38-02.png)](/images/2022-09-26_17-38-02.png)

**セキュリティの既定値**が**有効化**されています。以下の手順に従い、**無効化**します。

1. [Azureポータル](https://portal.azure.com){target=_blank}へ全体管理者でログインします。
2. **Azure Active Directory**管理画面に移動します。（画面上部の検索ボックスに**Active**と入力し、表示された**Azure Active Directory**をクリックします。）

    [![Screenshot](/images/2022-09-26_7-46-39.png)](/images/2022-09-26_7-46-39.png)

3. サイドメニューから**プロパティ**をクリックします。
4. **セキュリティの既定値の管理**をクリックします。
5. **セキュリティの既定値の有効化**を**いいえ**を選択し、**保存**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_17-39-08.png)](/images/2022-09-26_17-39-08.png)

#### M365のログイン画面で、詳細情報が必要の画面が表示された場合

[![Screenshot](/images/2022-09-26_18-11-23.png)](/images/2022-09-26_18-11-23.png)

M365の**パスワード リセットのセルフサービス**が**有効**となっています。SingleIDで認証するユーザに対して、M365のセルフパスワードリセットを**無効化**します。

1. [Azureポータル](https://portal.azure.com){target=_blank}へ全体管理者でログインします。
2. **Azure Active Directory**管理画面に移動します。（画面上部の検索ボックスに**Active**と入力し、表示された**Azure Active Directory**をクリックします。）

    [![Screenshot](/images/2022-09-26_7-46-39.png)](/images/2022-09-26_7-46-39.png)

3. サイドメニューから**ユーザ**をクリックします。
4. サイドメニューから**パスワードリセット**をクリックします。
5. **パスワード リセットのセルフサービスが有効**を**すべて**から**選択済み**（シングルサインオンを実施しないユーザを選択）または**なし**に変更し、保存ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_18-50-27.png)](/images/2022-09-26_18-50-27.png)

## 補足
* [M365連携機能の仕様](./featuires.md)
* [M365連携のよくある質問](./faq.md)
