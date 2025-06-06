# IntuneによるSCEP証明書の配布手順

このドキュメントでは、Microsoft Intune を使って SCEP 証明書をデバイスに配布する手順を説明します。  
※ 本手順では、対象デバイスがすでに Intune に登録済みであることを前提としています。

## Microsoft Entra IDへアプリ登録
1. [Azureポータル](https://portal.azure.com){target=_blank}へグローバル管理者でログインします。
2. **Microsoft Entra ID**管理画面に移動します。（画面上部の検索ボックスに「**entra**」と入力し、表示された**Microsoft Entra ID**をクリックします。）

    [![Screenshot](/images/2025-05-10_18-05-38.png)](/images/2025-05-10_18-05-38.png)

3. サイドメニューから**アプリの登録**をクリックします。

    [![Screenshot](/images/2022-09-26_7-50-38.png)](/images/2022-09-26_7-50-38.png)

4. **新規登録**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_7-53-45.png)](/images/2022-09-26_7-53-45.png)

5. 以下の内容を設定し、**登録**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **名前** | 任意です。（例：SingleID） |
    | **サポートされているアカウントの種類** | この組織ディレクトリのみに含まれるアカウント (シングル テナント) |

    [![Screenshot](/images/2022-09-26_7-55-21.png)](/images/2022-09-26_7-55-21.png)

6. **アプリケーション (クライアント) ID**、**ディレクトリ (テナント) ID**は、[SingleID SCEPサーバのエンドポイントの作成](#singleid-scepサーバのエンドポイントの作成)で必要となります。**値をコピーして、保存**しておきます。

    [![Screenshot](/images/2022-09-26_8-53-32.png)](/images/2022-09-26_8-53-32.png)

7. サイドメニューから**証明書とシークレット**をクリックします。

    [![Screenshot](/images/2022-09-26_8-15-17.png)](/images/2022-09-26_8-15-17.png)

8. **クライアント シークレット**タブへ移動し、**新しいクライアント シークレット**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_8-18-49.png)](/images/2022-09-26_8-18-49.png)

9. 以下の内容を設定し、**追加**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **説明** | 任意です。（例：SingleID連携） |
    | **有効期限** | 24か月 |

    [![Screenshot](/images/2022-09-26_8-28-54.png)](/images/2022-09-26_8-28-54.png)

10. 作成された**クライアント シークレット**は、[SingleID SCEPサーバのエンドポイントの作成](#singleid-scepサーバのエンドポイントの作成)で必要となります。**値をコピーして、保存**しておきます。

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
15. 以下のアクセス許可を追加します。選択したら**アクセス許可の追加**ボタンをクリックします。

    * Application.Read.All

16. **アクセス許可の追加**ボタンをクリックします。

    [![Screenshot](/images/2022-09-26_9-45-35.png)](/images/2022-09-26_9-45-35.png)

17. **Microsoft API**の**Intune**を選択します。

    [![Screenshot](/images/2025-05-10_18-53-31.png)](/images/2025-05-10_18-53-31.png)

18. **アプリケーションの許可**を選択します。
19. 以下のアクセス許可を追加します。選択したら**アクセス許可の追加**ボタンをクリックします。

    * scep_challenge_provider

20. 追加したアクセス許可が一覧に表示されたことを確認し、**管理者の同意を与えます**ボタンをクリックします。

    [![Screenshot](/images/2022-09-28_15-40-18.png)](/images/2022-09-28_15-40-18.png)

21. **はい**をクリックします。

    [![Screenshot](/images/2022-09-26_11-52-42.png)](/images/2022-09-26_11-52-42.png)


## SingleID SCEPサーバのエンドポイントの作成
SingeID SCEPサーバのエンドポイントを作成するために以下の情報を**SingleIDテクニカルサポート**に提供してください。

* SingleIDのアカウント番号
* アプリケーション (クライアント) ID
* ディレクトリ (テナント) ID
* クライアント シークレットの値

## プラットフォームごとの構成プロファイルの作成方法

!!! info
 
    * Entra ID に存在するユーザを、SingleID 側にもあらかじめ作成しておくこと。
    * SingleID のユーザ名は Entra ID UPN の “@” より前の部分 と一致させる（例：taro@example.com → taro）。

* [Windows 10 以降](windows10later.md)
* [Mac](mac.md)
* Linux（予定なし）
* [Android](android.md)
* [iOS/iPadOS](ios_ipados.md)

## トラブルシューティング
### Windows デバイスのログ確認  
[マイクロソフトの公式サイト](https://learn.microsoft.com/ja-jp/troubleshoot/mem/intune/certificates/troubleshoot-scep-certificate-profiles#logs-for-windows-devices){target=_blank}

### Android デバイスのログ確認  
[マイクロソフトの公式サイト](https://learn.microsoft.com/ja-jp/troubleshoot/mem/intune/certificates/troubleshoot-scep-certificate-profiles#logs-for-android-devices){target=_blank}

### iOS/iPadOS デバイスのログ確認  
[マイクロソフトの公式サイト](https://learn.microsoft.com/ja-jp/troubleshoot/mem/intune/certificates/troubleshoot-scep-certificate-profiles#logs-for-ios-and-ipados-devices){target=_blank}

### macOS デバイスのログ確認

ターミナルから以下のコマンドを実行します。

```
% log show --debug --info | grep "com.apple.ManagedClient:SCEP"
```
