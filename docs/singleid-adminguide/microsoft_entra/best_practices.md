# MicrosoftクラウドとSingleIDの連携

文書更新日:2026-08-02

本書は、Microsoft Entra IDで管理するユーザーとグループをSingleIDへ同期し、SingleIDが発行するクライアント証明書をアプリ認証とネットワーク認証に使用する場合の設計と運用を説明します。

個別の設定手順は、[関連する設定ガイド](#関連する設定ガイド)を参照してください。

## 推奨構成

推奨構成では、Microsoft Entra IDをユーザーとグループの管理元とします。アプリの認証はMicrosoft Entra CBA、ネットワーク機器の認証はSingleIDのクラウドRADIUSで行います。

[![Microsoft Entra IDとSingleIDを組み合わせた認証構成](/images/microsoft-entra-singleid-auth-architecture.png)](/images/microsoft-entra-singleid-auth-architecture.png)

| 機能 | 役割 |
| :-- | :-- |
| Microsoft Entra ID | ユーザー情報、グループ、在籍状態を管理し、Microsoft Entra CBAでアプリの認証を行います。 |
| SCIMプロビジョニング | Microsoft Entra IDのユーザーとグループをSingleIDへ同期します。 |
| Microsoft Intune | SingleIDのCA証明書、SCEP証明書プロファイル、Windows向けWi-Fi構成プロファイルを管理デバイスへ配布します。 |
| SingleID PKI/SCEP | アプリ認証とネットワーク認証に使用するクライアント証明書を発行し、失効情報をCRLで公開します。 |
| SingleIDクラウドRADIUS | ネットワーク機器から受信したEAP-TLS認証を処理し、RADIUSサイトに設定したユーザまたはグループの利用を許可します。 |
| ネットワーク機器 | 端末から802.1Xなどの認証要求を受け付け、SingleIDへRADIUS認証を要求します。認証後のVLAN、アクセス範囲、フィルタなどはネットワーク機器側で制御します。 |
| アプリ | Microsoft Entra IDの認証結果に基づいて利用を許可します。アプリの割り当てやアクセス制御はMicrosoft Entra IDおよび各アプリで設定します。 |

## 認証先の分担

アプリとネットワークでは、同じクライアント証明書を使用しますが、認証先が異なります。

| 対象 | 認証先 | 認証方式 | 確認するログ |
| :-- | :-- | :-- | :-- |
| Microsoft Entra IDと連携するアプリ | Microsoft Entra ID | Microsoft Entra CBA | Microsoft Entra IDのサインイン ログ |
| 無線LAN、有線LANなどのネットワーク | SingleIDクラウドRADIUS | RADIUSによるEAP-TLS認証 | SingleIDのRADIUS認証ログ |

アプリ認証では、Microsoft Entra IDがSingleIDのCAとCRLを使用してクライアント証明書を検証します。ネットワーク認証では、ネットワーク機器がSingleIDへRADIUS認証を要求し、SingleIDがクライアント証明書とユーザ・グループを確認します。

!!! info
    Microsoft Entra IDのパスワードは、SCIMでSingleIDへ同期されません。アプリの認証情報はMicrosoft Entra IDで管理します。SingleIDによるネットワーク認証ではクライアント証明書を使用するため、Microsoft Entra IDとSingleIDのパスワードを一致させる必要はありません。

## ユーザー属性の設計

SingleIDへ同期するユーザーについて、Microsoft Entra IDで以下の属性を設定します。

| Microsoft Entra IDの属性 | Microsoft Entra IDでの設定 | SingleIDへの反映 |
| :-- | :-- | :-- |
| ユーザー プリンシパル名（UPN） | SingleIDに登録したUPNサフィックスを使用します。同期開始後は変更しません。同期対象にできるUPNサフィックスは1つです。 | UPNの`@`より前の部分がSingleIDのユーザ名になります。 |
| メールアドレス | UPNと同じ値を設定します。また、ユーザーが受信できるメールアドレスであることを確認します。 | SingleIDのメールアドレスおよびSCEP証明書のサブジェクト代替名になります。 |
| 姓 | 同期対象ユーザーの姓を設定します。 | SingleIDの姓になります。 |
| 名 | 同期対象ユーザーの名を設定します。 | SingleIDの名になります。 |
| グループ | 同期対象グループへユーザーを直接所属させます。ネストされたグループのメンバーは同期されません。 | SingleIDへグループとメンバーが同期され、RADIUSサイトの許可グループとして使用できます。 |

## グループの設計

Microsoft Entra IDでは、次の対象をグループで管理します。

* SingleIDへSCIM同期するユーザーとグループ
* SCEP証明書を配布するユーザーまたはデバイス
* ネットワーク利用を許可するユーザー
* Microsoft Entra CBAの対象グループ

同じグループを複数の用途に使用できますが、同期、証明書配布、ネットワーク利用の対象を個別に変更する場合は、用途ごとにグループを分けます。検証用と本番用のグループも分け、少数のユーザーとデバイスで動作確認してから対象を拡大します。

SingleIDでは、SCIMで同期されたグループをRADIUSサイトの許可グループに設定します。ネットワーク機器側では、認証後に許可するVLANやアクセス範囲を別途設定します。

## ユーザー・デバイスに関する操作

| 場面 | Microsoft Entra ID / Intuneで行う操作 | SingleIDで行う操作 |
| :-- | :-- | :-- |
| 利用開始 | UPN、メールアドレス、姓、名を設定し、ユーザーをSCIM同期、Microsoft Entra CBA、ネットワーク利用の対象グループへ追加します。Intuneでは、CA証明書、SCEP証明書、Wi-Fi構成プロファイルを対象ユーザーまたはデバイスのグループへ割り当てます。 | 個別のユーザ登録は不要です。ユーザとグループはSCIMで同期され、クライアント証明書はSCEPで発行されます。 |
| 異動・権限変更 | 対象グループへの所属を変更します。 | 個別の変更は不要です。グループの変更はSCIMで同期されます。 |
| 退職・利用停止 | ユーザーを削除します。 | SCIMにより、Microsoft Entra IDでの論理削除時にユーザが無効化され、物理削除時にユーザが削除され、関連する証明書が失効します。論理削除後すぐに証明書の利用を停止する場合は、発行済みのクライアント証明書を失効させます。 |
| デバイス紛失・交換 | Intuneで該当デバイスをワイプまたは廃止します。 | 該当デバイスへ配布したクライアント証明書を失効します。 |

!!! warning
    SingleIDのユーザを無効化しても、発行済みのクライアント証明書は失効しません。ユーザを削除すると、そのユーザに関連する証明書はすべて失効します。

## 監視と定期確認

以下を定期的に確認します。

* Microsoft Entra IDのプロビジョニングログで、SCIM同期の失敗や遅延がないこと
* Microsoft Intuneで、CA証明書、SCEP証明書プロファイル、Wi-Fi構成プロファイルの配布に失敗していないこと
* Microsoft Entra IDのサインイン ログで、CBAの失敗や不審なサインインが増加していないこと
* SingleIDのRADIUS認証ログで、認証失敗や不審な認証試行が増加していないこと
* SingleIDで、期限切れが近い証明書、失効した証明書、ユーザに紐づく証明書を確認できること

## 関連する設定ガイド

* [Microsoft Entra ID（SCIM）によるSingleIDユーザ／グループ同期](scim.md)
* [IntuneによるSCEP証明書の配布手順](intune/index.md)
* [RADIUS](../radius.md)
* [RADIUS認証ログ](../radius_authlog.md)
* [Microsoft Entra CBA（SingleIDクライアント証明書）](cba.md)

## 参考情報

* [Microsoft Entra IDのアプリケーション プロビジョニングの動作](https://learn.microsoft.com/ja-jp/entra/identity/app-provisioning/how-provisioning-works){target=_blank}
* [Microsoft IntuneでSCEP証明書プロファイルを作成して割り当てる](https://learn.microsoft.com/ja-jp/intune/device-configuration/certificates/scep-profiles){target=_blank}
* [Microsoft Entra証明書ベース認証を設定する](https://learn.microsoft.com/ja-jp/entra/identity/authentication/how-to-certificate-based-authentication){target=_blank}
