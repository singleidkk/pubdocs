# Singleid Health for Firewall
## 本サービスの概要
本検査の目的は、検査対象システムに対してリモートから脆弱性の検査を行い、システムに存在する脆弱性を検出することにあります。 また、脆弱性が検出された場合、そのリスク評価、及び、脆弱性への対策を支援する情報の提供も行います。

## 実施までの流れ
1. （お客様）[**事前確認事項**](#事前確認事項)の全てに該当していることを確認します。
2. （お客様）[**注意事項**](#注意事項)を確認します。
3. （お客様）[**セキュリティ診断申請書**](#セキュリティ診断申請書)を記入します。
4. （お客様）セキュリティ診断の**申請**をします。
5. （SingleID）セキュリティ診断の**実施日時を決定**し、セキュリティ診断の**申請を受諾**します。
6. （SingleID）診断に必要な**ポイントを消費**します。
    
    !!! info
        ファイアウォールバンドルパックの場合には、年１回のセキュリティ診断が含まれているため、年１回のセキュリティ診断にはポイントを消費しません。

7. （SingleID）実施日に、**セキュリティ診断を実施**します。
8. （SingleID）**管理者ポータル画面＞セキュリティ診断**画面に**診断結果レポートを掲載**します。

    !!! info
        診断実施から診断結果レポート掲載まで、**3営業日**程度を目安としてください。

## 事前確認事項
診断可能なファイアウォールは、以下の**全ての項目に該当**している必要があります。

* 診断対象のファイアウォールのWAN側インターフェースが、**グローバルIPアドレス**を持っている。
* 診断対象のファイアウォールは**お客様所有の機器**である。サービス利用している場合には、サービス提供会社へ**セキュリティ診断実施の許可**を得ている。
* グルーバルIPアドレスが**固定IPアドレス**である。固定IPアドレスではない場合は、**ダイナミックDNSサービス**を利用して**名前解決が可能**である。

## 注意事項
* ファイアウォールが高負荷の場合には、セキュリティ診断が正常に終了しない場合があります。**インターネットの利用が多い時間帯を避けてセキュリティ診断の希望実施日を指定してください。**
* ファイアウォールが応答不能に陥った場合には、ファイアウォールの再起動が必要となります。**ファイアウォールの再起動の方法を事前に確認しておいてください。**

## セキュリティ診断申請書
以下のボタンをクリックして、セキュリティ診断申請書をダウンロードします。

[セキュリティ診断申請書のダウンロード](./application_sheets/singleid_health_for_fIrewall_application_sheet.xlsx){ target=_blank .md-button .md-button--primary }