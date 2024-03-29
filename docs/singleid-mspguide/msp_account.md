## 本ガイドの対象
SingleIDの特権MSPアカウントの管理者（SingleIDのディストリビュータ）を対象としています。

**SingleID MSP管理者ポータル＞管理アカウント＞MSPアカウント一覧**画面では、以下の操作を行うことができます。

* MSP/サポートアカウントの登録
* MSP/サポートアカウント情報の変更
* MSP/サポートアカウントの保有ポイントの変更
* MSP/サポートアカウントの解約

## MSP/サポートアカウント作成までの流れ
1. （特権MSPアカウントの管理者）アカウント登録

    1. **登録**ボタンをクリックします。**基本情報登録**画面がポップアップします。
    2. **顧客情報**および**契約ユーザ数**を入力し、登録ボタンをクリックします。

        !!! info
            顧客情報は、MSPアカウントが作成された後に、**SingleID 管理者ポータル＞管理＞基本情報**画面からお客様自身で変更することもできます。
        
        !!! warning
            契約ユーザを１０ユーザより多く設定した場合でも、１０ユーザのアカウントが作成されます。

    3. 登録されたMSPアカウントは、**SingleID MSP管理者ポータル＞管理アカウント＞承認待ちアカウント一覧**画面に表示されます。

2. （SingleID）登録申請されたMSPアカウントの情報及び契約ユーザ数を確認し、**承認**します。
3. （SingleID）システムへMSPアカウントを**プロビジョニング**します。

    !!! warning
        MSPアカウントの**承認**および**プロビジョニング**には、**原則3営業日**いただいております。余裕をもって申請いただくことをお勧めします。**至急、対応が必要な場合には、別途、担当営業までご相談ください。**

4. （特権MSPアカウントの管理者）プロビジョニング後、**MSP/サポートアカウント**に、**管理者**を登録します。

    1. 画面上部の**顧客画面**ボタンをクリックし、SingleID 管理者ポータルへ切り替えます。
    2. 左サイドメニューの上部にある**顧客切り替え**から、登録した**MSPアカウント名を選択**し、MSPアカウントの**SingleID 管理者ポータル**へアクセスします。
    3. **SingleID 管理者ポータル＞管理＞管理者**画面へ移動します。
    4. **登録**ボタンをクリックし、MSPアカウントに**管理者を登録**します。

5. （特権MSPアカウントの管理者）MSP/サポートアカウントへ**必要なポイントを追加**します。
6. （特権MSPアカウントの管理者）販売店へMSP/サポートアカウント情報および管理者情報をご案内します。

## 現在のポイントの変更
MSP/サポートアカウントが保有する**ポイント数**を**変更**します。MSPアカウントの**ポイント数の増減**が可能です。

!!! danger
    現在のポイントを**マイナス**にすることもできます。マイナスの状態は、**ポイントが不足**していることを示しています。**現在のポイントがマイナスの状態が続いた場合には、SingleIDをご利用できなくなりますので、ご注意ください**。

1. 変更したいMSPアカウントの**現在のポイント**の列にある**変更**ボタンをクリックします。**ポイント数更新**画面がポップアップします。
2. **ポイントを追加したい場合**には、**追加**を選択し、**ポイントを削除したい場合**には、**削除**を選択します。
3. **追加**または**削除**したい**変更ポイント**を入力します。
4. **変更**ボタンをクリックします。

## 解約処理
MSP/サポートアカウントの管理者からSingleIDを解約する依頼を受けた場合には、特権MSP管理者によって解約処理を実施します。解約する方法は、2通りあります。

1. **解約**ボタンをクリックして、**即時解約**する。
2. **解約予定日**を設定して、**解約予約**する。

!!! danger
    特権MSP/MSP/サポートアカウントの最小契約期間は１年間です。途中解約した場合には、残りの期間のポイント分を補充した上で解約するようにお願いします。（自動化を機能追加する予定です。）

### 即時解約
即時に解約する場合には、以下の手順を実施します。

1. 解約したいMSP/サポートアカウントの**操作**列の**解約**ボタンをクリックします。**解約**画面がポップアップします。
2. 「**解約処理が完了する際に、現在のポイントの不足分のポイントは補充され、残存分のポイントは消費されます。解約を続行しますか？**」と聞かれます。
3. 内容に問題がなく、**解約処理を続行する場合**には、**同意する**にチェックをし、**解約**ボタンをクリックします。
4. MSP/サポートアカウントが解約され、MSP/サポートアカウントの列がグレーに変わります。

    !!! info
        同意したように、**現在のポイント**の**不足分のポイントは補充**され、**残存分のポイントは消費**されています。

### 解約予約
解約を予約する場合には、以下の手順を実施します。

1. 解約したいMSPアカウントの**解約予定日**列の**設定**ボタンをクリックします。**解約予定日設定**画面がポップアップします。
2. 「**解約処理が完了する際に、現在のポイントの不足分のポイントは補充され、残存分のポイントは消費されます。解約を続行しますか？**」と聞かれます。
3. 内容に問題がなく、**解約予定日に解約する場合**には、**同意する**にチェックをし、**解約**ボタンをクリックします。
4. **解約予定日**が設定され、解約予定日の列に日付が表示されます。
5. 設定した**解約予定日**に、**解約処理が自動で実行**されます。

    !!! info
        解約処理が自動実行された際には、同意したように、**現在のポイント**の**不足分のポイントは補充**され、**残存分のポイントは消費**されます。
