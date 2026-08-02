# Microsoft Entra CBA（SingleIDクライアント証明書）

文書更新日:2026-08-02

本書では、SingleIDクライアント証明書を使用して、Microsoft Entra CBA（証明書ベース認証）を設定する手順を説明します。

!!! info
    * MicrosoftのFAQでは、Microsoft Entra CBAは無料の機能であり、Microsoft Entra IDのすべてのエディションに含まれると案内されています（2026-08-02参照）。最新のライセンス要件は、[Microsoft Entra CBA FAQ（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/certificate-based-authentication-faq){target=_blank}を確認してください。
    * 本書では、SingleIDクライアント証明書のサブジェクト代替名（SAN）に設定されたメールアドレスを、Microsoft Entra IDのユーザー プリンシパル名（UPN）へ対応付けます。

本書では、SingleIDクライアント証明書を単一要素認証として使用します。パスワードとSingleIDクライアント証明書を組み合わせる構成の検証情報は、[Microsoft Entra 2要素認証（パスワード＋SingleIDクライアント証明書）](cba_mfa.md)を参照してください。

## 設定概要

| **設定対象** | **設定内容** |
| :--- | :--- |
| **Microsoft Entra IDの信頼するCA** | SingleIDのルートCAと中間CA |
| **中間CAの設定** | SingleIDの中間CAに表示される**失効リストの配布ポイント**をCRLのURLに指定し、**発行者ヒント**を有効化 |
| **CBAの対象** | 有効なSingleIDクライアント証明書を利用できるテストユーザーのグループ |
| **CRL検証** | **CRL 検証を必須にする（推奨）**を有効化し、SingleIDのルートCAのみを検証対象から除外 |
| **認証バインド** | **単一要素認証**、**低アフィニティ バインド** |
| **ユーザー名バインド** | 証明書フィールドの**RFC822Name**をユーザー属性の**userPrincipalName**にバインド |

## 前提条件

* Microsoft Entra IDのユーザー プリンシパル名（UPN）と、SingleIDユーザのメールアドレスが一致していること
* Microsoft Entra CBAを利用するユーザーを登録したMicrosoft Entra IDのグループがあること
* Microsoft Entra IDのグローバル管理者アカウントを利用できること
* 有効なSingleIDクライアント証明書を、利用するデバイスへインストールできること

!!! warning
    最初からすべてのユーザーをCBAの対象にしないでください。クライアント証明書を利用できるテストユーザーのグループから段階的に適用してください。また、CBA設定の影響を受けない緊急アクセス用アカウントを用意してください。

## SingleIDのCA証明書とCRL情報の取得

1. **SingleID 管理者ポータル＞認証＞証明書**画面の**基本情報**タブへ移動します。
2. **ルートCA**のダウンロードアイコンをクリックし、ルートCA証明書をダウンロードします。
3. **中間CA**のダウンロードアイコンをクリックし、中間CA証明書をダウンロードします。
4. **中間CA**に表示されている**失効リストの配布ポイント**を記録します。Microsoft Entra IDへ中間CAを登録するときに、証明書失効リスト（CRL）のURLとして使用します。

## クライアント証明書の発行

### 証明書プロファイルの確認

1. **SingleID 管理者ポータル＞認証＞証明書**画面の**プロファイル**タブへ移動します。
2. クライアント証明書の発行に使用するプロファイルで、以下の項目を確認します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **拡張キー使用法（EKU）** | **クライアント認証** |
    | **サブジェクトの代替名** | **メールアドレス** |

### ユーザへの証明書発行

1. **SingleID 管理者ポータル＞ユーザ一覧**画面へ移動します。
2. 対象ユーザのメールアドレスが、Microsoft Entra IDのUPNと一致していることを確認します。
3. 対象ユーザの行にある**チェックボックス**を選択します。
4. **選択実行**ボタンの▼をクリックし、**証明書の発行**を選択します。
5. CBA用の証明書プロファイルを選択し、**発行**ボタンをクリックします。
6. 発行されたPKCS#12形式のクライアント証明書を、利用するデバイスへインストールします。

Windowsへのインストール手順は、[Windows: クライアント証明書のインストール](../../singleid-pocguide/clients/windows_install_cert.md)を参照してください。

## Microsoft Entra IDへのCA登録

1. [Microsoft Entra 管理センター](https://entra.microsoft.com/){target=_blank}へ、グローバル管理者でログインします。
2. **Entra ID＞セキュリティ＞管理＞公開キー基盤**画面へ移動します。
3. **PKIの作成**ボタンをクリックします。
4. **表示名**に任意の名前（例：`SingleID PKI`）を入力し、**作成**ボタンをクリックします。
5. 作成したPKIを開き、**証明機関の追加**ボタンをクリックします。
6. SingleIDのルートCA証明書を選択し、以下を設定して保存します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ルート証明書** | **はい** |
    | **発行者ヒント** | **無効** |

7. 再度**証明機関の追加**ボタンをクリックし、SingleIDの中間CA証明書を選択します。
8. 中間CAを追加するときは、以下を設定して保存します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **ルート証明書** | **いいえ** |
    | **証明書失効リストのURL** | **SingleID 管理者ポータル＞認証＞証明書＞基本情報**タブの**中間CAの失効リストの配布ポイント** |
    | **発行者ヒント** | **有効** |

    SingleIDの中間CAを発行者ヒントの対象にし、CBAの構成画面でも発行者ヒントを有効にすると、接続デバイスへSingleIDの中間CA情報が送信され、証明書選択画面に表示するクライアント証明書を絞り込めます。発行者ヒントは、証明書の信頼性や失効を検証する設定ではありません。

!!! warning
    * Microsoft Entra IDがCRLへアクセスできない場合、CBAによる認証は失敗します。CRLのURLは、インターネットから参照できるURLを設定してください。
    * Microsoft Entra IDへCAを登録すると、そのCAが発行した証明書がCBAの信頼対象になります。CBA用の証明書プロファイルと証明書の発行対象を適切に管理してください。

## Microsoft Entra CBAの有効化

1. Microsoft Entra CBAを利用するテストユーザーを、CBA対象グループへ追加します。
2. [Microsoft Entra 管理センター](https://entra.microsoft.com/){target=_blank}へ、グローバル管理者でログインします。
3. **Microsoft Entra 管理センター＞Entra ID＞セキュリティ＞管理＞認証方法＞証明書ベースの認証**画面へ移動します。
4. **有効化とターゲット**でCBAを有効にします。
5. **グループの選択**でCBA対象グループを追加します。
6. 確認項目を選択し、**保存**ボタンをクリックします。

!!! warning
    CBAの対象ユーザーは、Microsoft Entra ID上でMFAを実行可能なユーザーとして扱われます。有効なクライアント証明書を利用できないユーザーを対象にすると、MFAを完了できず、ほかの認証方法の登録やサインインができなくなる場合があります。CBAの対象は、有効なクライアント証明書を利用できるユーザーに限定してください。

## Microsoft Entra CBAの構成

1. **Microsoft Entra 管理センター＞Entra ID＞セキュリティ＞管理＞認証方法＞証明書ベースの認証**画面で、**構成**をクリックします。
2. **CRL 検証を必須にする（推奨）**を有効にします。
3. **CA を CRL 検証から除外する**で、SingleIDのルートCAを追加します。
4. **発行者ヒント**を有効にします。
5. 認証バインドを以下のように設定します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **保護レベル** | **単一要素認証** |
    | **アフィニティ バインド** | **低** |

6. ユーザー名バインドへ以下の規則を追加します。

    | **証明書フィールド** | **ユーザー属性** |
    | :--- | :--- |
    | **RFC822Name** | **userPrincipalName** |

7. 設定を保存します。

!!! info
    * SingleIDの中間CAは、CRL検証から除外しないでください。失効したクライアント証明書は、中間CAのCRLを使用して検証されます。
    * SingleIDクライアント証明書のSANに設定されたメールアドレスと、Microsoft Entra IDのUPNが一致するユーザーを認証します。一致しない場合、CBAによるサインインは失敗します。
    * Microsoft Entra IDでは認証方法ポリシーがキャッシュされるため、CBA設定の変更が反映されるまでに最大1時間かかる場合があります。設定直後に動作を確認できない場合は、時間をおいて再度確認してください。

!!! warning
    本書では、PKCS#12形式で配布するクライアント証明書を単独で使用するため、**単一要素認証**に設定します。

## 動作確認

1. CBA対象ユーザーのクライアント証明書がインストールされたデバイスで新しいブラウザー セッションを開始し、[マイ アプリ ポータル](https://myapps.microsoft.com/){target=_blank}へアクセスします。
2. Microsoft Entra IDのUPNを入力し、**次へ**ボタンをクリックします。
3. **証明書を使用してサインイン**を選択します。
4. SingleIDクライアント証明書を選択します。
5. マイ アプリ ポータルへサインインできることを確認します。
6. **Microsoft Entra 管理センター＞Entra ID＞監視＞サインイン ログ**画面で、対象ユーザーの認証結果と証明書情報を確認します。

## 運用上の注意

* SingleIDユーザを無効化しても、発行済みのクライアント証明書は失効しません。CBAによるサインインを停止する場合は、SingleIDでクライアント証明書を失効してください。
* クライアント証明書を更新した場合は、新しい証明書を利用するデバイスへインストールしてください。
* CBA対象グループ、証明書の発行状況、証明書の有効期限および失効状況を定期的に確認してください。
* Microsoft Entra CBAでは、通常のサインイン先である`login.microsoftonline.com`とは別に、クライアント証明書を提示するための`*.certauth.login.microsoftonline.com`を使用します。プロキシやファイアウォールで通信先を制限している場合は、このドメインへのHTTPS通信も許可してください。また、TLS検査を行うとクライアント証明書のやり取りが成立しない場合があるため、TLS検査の対象から除外してください。通信先制限やTLS検査を使用していない場合、追加設定は不要です。

## 参考情報

* [Microsoft Entra CBAを設定する（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/how-to-certificate-based-authentication){target=_blank}
* [Microsoft Entra証明書ベース認証用に証明機関を構成する（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/how-to-configure-certificate-authorities){target=_blank}
* [Microsoft Entra CBAの概要（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/concept-certificate-based-authentication){target=_blank}
* [Microsoft Entra CBAに関するFAQ（Microsoft）](https://learn.microsoft.com/ja-jp/entra/identity/authentication/certificate-based-authentication-faq){target=_blank}
* [証明書の管理](../pki.md)
* [クライアント証明書の発行](../user.md#証明書の発行失効)
