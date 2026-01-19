# Microsoft Entra ID（SCIM）による SingleID ユーザー／グループ同期
文書更新日:2026-01-19

## 1. 目的と概要

本マニュアルは、Microsoft Entra ID（旧 Azure Active Directory）の **SCIM プロビジョニング機能** を利用して、**SingleID** にユーザーおよびグループを自動同期するための設定手順書です。

本書は以下の利用者を想定しています。

* Microsoft Entra ID 管理者
* SIer / 販売店経由で SingleID を導入するエンドユーザー管理者

※ 運用ポリシーや障害対応フローは本書には含めません。

---

## 2. 前提条件

この手順を実施する前に、以下を確認してください。

### 必要な権限・準備

* Microsoft Entra ID（Microsoft Entra テナント）が利用可能であること
* Microsoft Entra ID にて、**エンタープライズ アプリケーションの作成**および**プロビジョニング設定**ができる権限を有していること
* SingleID テクニカルサポートへ連絡できること（シークレット トークン発行のため）
* SingleID のアカウント番号、Microsoft Entra ID のテナント ID を確認できること

### UPN の運用（重要）

SingleID では、Microsoft Entra ID の**ユーザー プリンシパル名（UPN）**を元にユーザーを識別します（SingleID 側のユーザー名は UPN の`@`より前の部分が利用されます）。

* 同期対象ユーザーの UPN の`@`より前の部分が、テナント内で重複していないこと

    例：`user1@a.example` と `user1@b.example` は重複扱い
* 同期開始後に、同期対象ユーザーの UPN を変更しない運用にできること

!!! warning
    上記が満たせない場合、SCIM 連携が正常に動作しない（または意図しないユーザーに紐づく）可能性があります。

### 対応エディション（Microsoft Entra ID）

SingleID の SCIM 連携機能は、以下の Microsoft Entra ID エディションを前提として設計・検証されています。

* Microsoft Entra ID P1
* Microsoft Entra ID P2
* Microsoft Entra ID Governance
* Microsoft Entra Suite（P1 / P2 機能を含む）

以下は動作保証外（サポート対象外）です。

* Microsoft Entra ID Free

参考（Microsoft Learn）

* [Microsoft Entra ID ライセンス比較](https://learn.microsoft.com/ja-jp/entra/fundamentals/licensing){target=_blank}
* [How application provisioning works in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/app-provisioning/how-provisioning-works){target=_blank}

---

## 3. 全体構成（参考）

```
Microsoft Entra ID（SCIM クライアント）
        |
        |  SCIM 2.0 (HTTPS)
        v
SingleID SCIM エンドポイント
        |
        v
SingleID ユーザー／グループ管理
```

---

## 4. SingleID 側の設定（SCIM エンドポイント用のシークレット トークン発行）

SingleID の SCIM 連携は、必要なパラメータを SingleID 側へ提示いただき、SingleID 側で SCIM エンドポイント用のシークレット トークンを発行します。

### 4.1 SingleID テクニカルサポートへ提供する情報

以下の情報を **SingleID テクニカルサポート**へ提供してください。

* SingleID のアカウント番号
* Microsoft Entra ID のテナント ID

### 4.2 SingleID から提供される情報

SingleID から、以下の情報が提供されます（Microsoft Entra ID 側の設定で利用します）。

* **シークレット トークン**

#### SCIM エンドポイントURL（環境別）

* PoC環境：`https://scim-02-0001.poc.singleid.jp/scim/v2`
* 本番環境：`https://scim-00-0001.service.singleid.jp/scim/v2`

!!! warning
    シークレット トークンを再発行した場合、既存のシークレット トークンは利用できなくなります。運用中に再発行する際は、Microsoft Entra ID 側の設定も更新してください。

---

## 5. Microsoft Entra ID 側の設定

### 5.1 エンタープライズ アプリケーションの作成

1. [Azureポータル](https://portal.azure.com){target=_blank}へ管理者でログインします。
2. **Microsoft Entra ID**管理画面に移動します。（画面上部の検索ボックスに「**entra**」と入力し、表示された**Microsoft Entra ID**をクリックします。）
3. サイドメニューから**エンタープライズ アプリケーション**をクリックします。
4. **新しいアプリケーション**ボタンをクリックします。
5. **独自のアプリケーションの作成**ボタンをクリックします。
6. アプリ名を入力します（例：`SingleID SCIM`）。
7. **ギャラリーに見つからないその他のアプリケーションを統合します (ギャラリー以外)**を選択します。
8. **作成**ボタンをクリックします。

---

### 5.2 プロパティの設定

1. 作成したアプリケーションを開きます。
2. サイドメニューから**プロパティ**をクリックします。
3. 以下を設定し、**保存**をクリックします。

    | 項目 | 設定値 |
    | --- | --- |
    | **ユーザーのサインインが有効になっていますか?** | **いいえ** |
    | **割り当てが必要ですか?** | **はい** |
    | **ユーザーに表示しますか?** | **いいえ** |

設定理由：

* 本アプリは SCIM 同期用途のため、エンドユーザーのサインインや表示は不要です。
* 同期対象をアプリに割り当てたユーザー／グループに限定するため、**割り当てが必要**は **はい**のままにします。

---

### 5.3 プロビジョニングの有効化

1. サイドメニューから**プロビジョニング**をクリックします。
2. **新しい構成**ボタンをクリックします。
3. **新しいプロビジョニング構成**画面で、以下を設定します。

    | 項目 | 設定値 |
    | --- | --- |
    | 認証方法の選択 | **ベアラー認証** |
    | テナントのURL | 本書記載の [**SCIM エンドポイントURL**](#4-singleid-側の設定scim-エンドポイント用のシークレット-トークン発行) |
    | シークレット トークン | SingleID から提供された **シークレット トークン** |
4. **テスト接続**ボタンをクリックし、接続テストが成功することを確認します。
5. **作成**ボタンをクリックします。
6. 作成完了後、アプリの**概要（プレビュー）**画面が表示されます。
7. 画面上部に**プロビジョニングの開始**ボタンが表示されていることを確認します。

!!! info
    **プロビジョニングの開始**は、属性マッピングとユーザー／グループの割り当てが完了してから実行してください。

---

## 6. 属性マッピング

### 6.1 ユーザー属性マッピング（Users）

マッピングは、サイドメニューの**属性マッピング（プレビュー）**から設定します。

1. サイドメニューから**属性マッピング（プレビュー）**をクリックします。
2. 一覧から **Provision Microsoft Entra ID Users** をクリックします。

Microsoft Entra ID のユーザー属性を SingleID の SCIM ユーザー属性にマッピングします。

SingleID 連携では、基本的に**デフォルトのマッピングのままで動作します**。以下はデフォルトのマッピング例です。

| customappsso 属性 | Microsoft Entra ID 属性 |
| --- | --- |
| userName | userPrincipalName |
| active | `Switch([IsSoftDeleted], , "False", "True", "True", "False")` |
| displayName | displayName |
| title | jobTitle |
| emails[type eq "work"].value | mail |
| preferredLanguage | preferredLanguage |
| name.givenName | givenName |
| name.familyName | surname |
| name.formatted | `Join(" ", [givenName], [surname])` |
| addresses[type eq "work"].formatted | physicalDeliveryOfficeName |
| addresses[type eq "work"].streetAddress | streetAddress |
| addresses[type eq "work"].locality | city |
| addresses[type eq "work"].region | state |
| addresses[type eq "work"].postalCode | postalCode |
| addresses[type eq "work"].country | country |
| phoneNumbers[type eq "work"].value | telephoneNumber |
| phoneNumbers[type eq "mobile"].value | mobile |
| phoneNumbers[type eq "fax"].value | facsimileTelephoneNumber |
| externalId | mailNickname |
| urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber | employeeId |
| urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department | department |
| urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager | manager |

---

### 6.2 グループ属性マッピング（Groups）

1. サイドメニューから**属性マッピング（プレビュー）**をクリックします。
2. 一覧から **Provision Microsoft Entra ID Groups** をクリックします。

Microsoft Entra ID のグループ属性を SingleID の SCIM グループ属性にマッピングします。

SingleID 連携では、基本的に**デフォルトのマッピングのままで動作します**。以下はデフォルトのマッピング例です。

| customappsso 属性 | Microsoft Entra ID 属性 |
| --- | --- |
| displayName | displayName |
| externalId | objectId |
| members | members |

**注意：**

* Microsoft Entra ID の SCIM では、グループの入れ子（グループのメンバーに別グループが含まれる状態）は**展開されません**。
* SingleID に同期されるのは、割り当てたグループの**直下のメンバー（直接メンバー）**のみです。

---

## 7. アプリケーションへの割り当て

### 7.1 ユーザー／グループの割り当て

1. サイドメニューから**ユーザーとグループ**をクリックします。
2. **ユーザーまたはグループの追加**ボタンをクリックします。
3. 同期したい **ユーザーまたはグループ** を選択します。
4. **割り当て**ボタンをクリックします。

> ⚠️ 割り当てられていないユーザー／グループは SCIM 同期されません。

!!! warning
    **グループを含むグループ（入れ子になっているグループ）**を割り当て対象に選択しないでください。SingleID に同期されるのは、割り当てたグループの**直下のメンバー（直接メンバー）**のみです。グループを割り当てる場合は、メンバーが**ユーザーのみ**のグループを選択してください。

---

### 7.2 プロビジョニングの開始

属性マッピングとユーザー／グループの割り当てが完了したら、プロビジョニングを開始します。

1. サイドメニューから**概要（プレビュー）**をクリックします。
2. 画面上部の**プロビジョニングの開始**ボタンをクリックします。
3. 画面上部の表示が**プロビジョニングを一時停止**に切り替わることを確認します。

---

## 8. プロビジョニング動作仕様

この章では、同期の動作（どの項目が同期されるか／どの操作が反映されるか）を説明します。

### 8.0 SingleIDへ同期されるユーザー項目

SingleID に同期される主なユーザー項目は以下です。

* ユーザー名：Microsoft Entra ID の**ユーザー プリンシパル名（UPN）**の`@`より前の部分
* 名：`givenName`
* 姓：`surname`
* メールアドレス：`mail`
* 携帯電話：`mobile`
* 勤務先の電話番号：`telephoneNumber`
* 郵便番号：`postalCode`
* 都道府県：`state`
* 市区町村：`city`
* 番地：`streetAddress`
* 部署：`department`
* 役職：`jobTitle`
* アカウントの有効/無効：`active`

### 8.1 同期のキー（重要）

SingleID では、Microsoft Entra ID から同期されるユーザーを **ユーザー プリンシパル名（UPN）** を元に識別します。SingleID 側のユーザー名は、UPN の `@` より前の部分が利用されます。

!!! warning
    UPN の `@` より前の部分が同じユーザーが複数いると、意図しないユーザーに紐づく可能性があります。UPN の運用ルール（重複しないこと）を確認してください。

!!! warning
    UPN を変更すると、SingleID 側のユーザー名と一致しなくなり不整合が発生します。運用上、同期対象ユーザーの UPN は変更しないでください。

### 8.2 同期される操作

| Microsoft Entra ID 操作 | SingleID 側の動作 |
| --- | --- |
| ユーザー追加 | ユーザー作成 |
| ユーザー属性変更 | ユーザー更新 |
| ユーザー割当解除 / ユーザー無効化 / ユーザー論理削除 | ユーザー無効化 |
| ユーザー物理削除 | ユーザー削除 |
| グループ作成 | グループ作成 |
| メンバー追加/削除 | グループメンバー更新 |

---

### 8.3 同期タイミング

* 初回同期：プロビジョニング有効化後に自動実行
* 定期同期／反映タイミング：Microsoft Entra ID 側の仕様に従います（最新情報は Microsoft の公式ドキュメントを参照してください）

参考：[Microsoft Entra ID のプロビジョニングに関する公式ドキュメント](https://learn.microsoft.com/ja-jp/entra/identity/app-provisioning/){target=_blank}

---

## 9. レート制限と処理遅延について（重要）

### 9.1 SingleID の基本方針

SingleID の SCIM 実装では、以下の方針を採用しています。

* **HTTP 429（Too Many Requests）は返しません**
* **キューイング処理は行いません**
* リクエストが集中した場合は、内部で **処理を遅延** させることで**処理速度を自動的に調整** します

この設計により、Microsoft Entra ID 側の SCIM クライアントがレート制限応答を解釈できない場合でも、
プロビジョニングジョブがエラーや隔離（Quarantine）状態になることを防ぎます。

---

### 9.2 一時的な障害時の挙動

| 状態 | SingleID の対応 |
| --- | --- |
| 一時的な高負荷 | 内部リトライ／遅延処理 |
| 上流 API 障害 | 503 / 504 を返却 |
| 致命的エラー | 500 を返却 |

---

## 10. トラブルシューティング（設定時）

### 10.1 よくある設定ミス

| 事象 | 原因 | 確認ポイント |
| --- | --- | --- |
| ユーザーが同期されない | 割り当て漏れ | アプリにユーザー／グループが割り当てられているか |
| メールが反映されない | 属性マッピング不備 | `mail → emails[type=work]` が設定されているか |
| グループメンバーが欠落 | ネストグループ | 直接メンバーのみが割り当てられているか |

---

## 11. 初回セットアップ時の推奨手順

* 最初は **少数ユーザー（1〜2名）で接続テスト** を行う
* 問題がなければ、段階的にグループ割り当てを追加する
* 大規模同期を行う前に、属性マッピング画面を必ず確認する
