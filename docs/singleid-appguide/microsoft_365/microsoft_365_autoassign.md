# Microsoft 365（M365）ライセンスの自動割当

## M365ライセンスの自動割当の制限

* **[自動割当可能なM365ライセンス一覧](#自動割当可能なm365ライセンス一覧)**にあるライセンスを契約していること
* **[自動割当可能なM365ライセンス一覧](#自動割当可能なm365ライセンス一覧)**にある複数種類のライセンスを契約していないこと（複数種類のライセンスを契約していた場合に、SingleIDでは、どのライセンスを割当てて良いかの判断ができません。）
* 利用可能なライセンス数があること

## M365ライセンスの自動割当の代替案
M365では、グループにライセンスを割当てることで、メンバーのユーザに同一のライセンスを割当てることができます。[M365ライセンスの自動割当の制限](#m365ライセンスの自動割当の制限)により、自動割当ができなかった場合には、以下の方法で、グループ同期で作成されたグループにライセンスを割当てることで、ユーザにライセンスを割当てることができます。

1. [Azureポータル](https://portal.azure.com){target=_blank}へグローバル管理者でログインします。
2. **Azure Active Directory**管理画面に移動します。（画面上部の検索ボックスに**Active**と入力し、表示された**Azure Active Directory**をクリックします。）

    [![Screenshot](/images/2022-09-26_7-46-39.png)](/images/2022-09-26_7-46-39.png)

3. サイドメニューから**グループ**をクリックします。
4. ライセンスを割当てたい**グループ名**をクリックします。
5. サイドメニューから**ライセンス**をクリックします。
6. **割り当て**ボタンをクリックします。
7. 割当てたい**ライセンスを選択**し、**保存**ボタンをクリックします。

## 自動割当可能なM365ライセンス一覧

ライセンスの情報については、[ライセンスのための製品名とサービス プラン 識別子](https://learn.microsoft.com/ja-jp/azure/active-directory/enterprise-users/licensing-service-plan-reference
){target=_blank}を参考にしています。

| 製品名 | 文字列 ID | GUID |
| :-- | :-- | :-- |
| Microsoft 365 Business Basic | O365_BUSINESS_ESSENTIALS |	3b555118-da6a-4418-894f-7df1e2096870 |
| Microsoft 365 Business Basic | SMB_BUSINESS_ESSENTIALS | dab7782a-93b1-4074-8bb1-0e61318bea0b |
| Microsoft 365 Apps for Business | O365_BUSINESS | cdd28e44-67e3-425e-be4c-737fab2899d3 |
| Microsoft 365 Apps for Business | SMB_BUSINESS | b214fe43-f5a3-4703-beeb-fa97188220fc |
| Microsoft 365 Business Standard | O365_BUSINESS_PREMIUM	| f245ecc8-75af-4f8e-b61f-27d8114de5f3 |
| Microsoft 365 Business Standard - Prepaid Legacy | SMB_BUSINESS_PREMIUM	| ac5cef5d-921b-4f97-9ef3-c99076e5470f |
| Microsoft 365 Business Premium | SPB | cbdc14ab-d96c-4c30-b9f4-6ada7cdc1d46 |
| Microsoft 365 E3 | SPE_E3	| 05e9a617-0261-4cee-bb44-138d3ef5d965 |
| Microsoft 365 E5 | SPE_E5	| 06ebc4ee-1bb5-47dd-8120-11324bc54e06 |
| 教職員用 Office 365 A1 | STANDARDWOFFPACK_FACULTY	| 94763226-9b3c-4e75-a931-5c89701abe66 |
| 学生用 Office 365 A1 | STANDARDWOFFPACK_STUDENT	| 314c4481-f395-4525-be8b-2ec4bb1e9d91 |
| 教職員用 Office 365 A1 Plus	| STANDARDWOFFPACK_IW_FACULTY	| 78e66a63-337a-4a9a-8959-41c6654dfb56 |
| 学生用 Office 365 A1 Plus	| STANDARDWOFFPACK_IW_STUDENT	| e82ae690-a2d5-4d76-8d30-7c6e01e6022e |
| 教職員用 Office 365 A3 | ENTERPRISEPACKPLUS_FACULTY	| e578b273-6db4-4691-bba0-8d691f4da603 |
| 学生用 Office 365 A3 | ENTERPRISEPACKPLUS_STUDENT	| 98b6e773-24d4-4c0d-a968-6e787a1f8204 |
| 教職員用 Office 365 A5 | ENTERPRISEPREMIUM_FACULTY | a4585165-0533-458a-97e3-c400570268c4 |
| 学生用 Office 365 A5 | ENTERPRISEPREMIUM_STUDENT | ee656612-49fa-43e5-b67e-cb1fdf7699df |
| Microsoft 365 E5 Developer (Windows および Audio Conferencing なし) | DEVELOPERPACK_E5 |	c42b9cae-ea4f-4ab7-9717-81576235ccac |

