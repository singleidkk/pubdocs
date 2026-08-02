# Microsoft Entra 2要素認証（パスワード＋SingleIDクライアント証明書）

文書更新日:2026-08-02

本書では、パスワードを第1要素、SingleIDクライアント証明書を第2要素とするMicrosoft Entra 2要素認証の検証手順を説明します。

Microsoftの技術資料では、パスワードを第1要素、単一要素認証として設定したMicrosoft Entra CBAを第2要素とする組み合わせが示されています。本書では、ユーザーごとのMFAを有効にし、対象ユーザーのモダン認証にMFAを要求します。

!!! warning
    本書は、パスワードとSingleIDクライアント証明書を組み合わせたMicrosoft Entra 2要素認証の検証手順です。Microsoft Entra IDのテナント設定、利用するアプリおよびデバイスにより、認証画面や選択できる認証方法が異なります。本番適用前に、利用環境で動作を確認してください。

!!! info
    * MicrosoftのFAQでは、Microsoft Entra CBAは無料の機能であり、Microsoft Entra IDのすべてのエディションに含まれると案内されています（2026-08-02参照）。最新のライセンス要件は、[Microsoft Entra CBA FAQ（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/certificate-based-authentication-faq){target=_blank}を確認してください。
    * Microsoftの資料では、Microsoft Entra ID FreeでもユーザーごとのMFAを利用できると案内されています（2026-08-02参照）。最新の利用条件は、[ユーザーごとのMicrosoft Entra多要素認証を有効にする（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/howto-mfa-userstates){target=_blank}を確認してください。
    * 対象アプリ、場所、デバイスなどの条件を指定してMFAを要求する場合は、Microsoft Entra ID P1またはP2の条件付きアクセスを利用してください。

## 前提条件

* [Microsoft Entra CBA（SingleIDクライアント証明書）](cba.md)の設定と動作確認が完了していること
* Microsoft Entra CBAの認証バインドで、証明書の**保護レベル**に**単一要素認証**が設定されていること
* Microsoft Entra IDのグローバル管理者アカウントを利用できること
* Microsoft Entra IDで**セキュリティの既定値群**を使用していないこと
* 対象ユーザーに、クライアント証明書以外のMFA方式を設定していないこと

!!! warning
    * 証明書の**保護レベル**を**多要素認証**に設定すると、証明書だけでMFA要件を満たす場合があります。パスワードとSingleIDクライアント証明書を組み合わせる本構成では、**単一要素認証**を設定してください。
    * **セキュリティの既定値群**は、テナント全体にMicrosoft所定のMFA要件などを適用する機能です。本書ではユーザーごとのMFAを使用するため、併用しません。
    * ユーザーごとのMFAは、MFAの実行を要求する機能です。第2要素としてCBAを指定する機能ではありません。対象ユーザーがほかのMFA方式を利用できる場合、クライアント証明書以外の認証方法が表示されることがあります。

## ユーザーごとのMFAの設定

1. [Microsoft Entra 管理センター](https://entra.microsoft.com/){target=_blank}へ、グローバル管理者でログインします。
2. **Microsoft Entra 管理センター＞Entra ID＞ユーザー＞すべてのユーザー**画面へ移動します。
3. **ユーザーごとのMFA**をクリックします。
4. CBAの動作確認が完了したテストユーザーを選択します。
5. **MFA を有効にする**をクリックします。
6. 確認画面で設定を確定します。

!!! warning
    最初からすべてのユーザーを対象にしないでください。パスワードとSingleIDクライアント証明書によるサインインを確認した後、対象ユーザーを段階的に追加してください。

## 動作確認

1. 対象ユーザーのクライアント証明書がインストールされたデバイスで、新しいブラウザー セッションを開始します。
2. [マイ アプリ ポータル](https://myapps.microsoft.com/){target=_blank}へアクセスします。
3. Microsoft Entra IDのUPNを入力し、**次へ**をクリックします。
4. パスワードを入力してサインインします。
5. 第2要素の認証方法として、証明書を使用する選択肢を選択します。
6. SingleIDクライアント証明書を選択します。
7. マイ アプリ ポータルへサインインできることを確認します。
8. **Microsoft Entra 管理センター＞Entra ID＞監視＞サインイン ログ**画面で、パスワードとMicrosoft Entra CBAによりMFA要件を満たしていることを確認します。

## 運用上の注意

* ユーザーごとのMFAは対象ユーザーのモダン認証に適用され、アプリごとに適用対象を変更できません。
* パスワードとMicrosoft Entra CBAを組み合わせた2要素認証は、レガシ認証を使用するアプリでは利用できません。
* ユーザーごとのMFAは、有効なクライアント証明書を利用できるユーザーだけに設定してください。
* SingleIDユーザを無効化しても、発行済みのクライアント証明書は失効しません。サインインを停止する場合は、SingleIDでクライアント証明書を失効してください。
* クライアント証明書の有効期限、失効状況および対象グループを定期的に確認してください。
* Microsoft Entra IDの仕様やテナント設定により認証画面が異なる場合があります。本番適用前に、実際に使用するアプリとデバイスで検証してください。
* Microsoftは、P1またはP2を利用できる場合は、ユーザーごとのMFAではなく条件付きアクセスによるMFAを推奨しています。

## 参考情報

* [Microsoft Entra CBAの技術的概念（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/concept-certificate-based-authentication-technical-deep-dive){target=_blank}
* [ユーザーごとの多要素認証を有効にする（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/howto-mfa-userstates){target=_blank}
* [Microsoft Entra多要素認証のライセンス（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/concept-mfa-licensing){target=_blank}
* [Microsoft Entra CBA（SingleIDクライアント証明書）](cba.md)
