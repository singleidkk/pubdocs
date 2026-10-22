# PHP、OpenJDK、Apache Tomcat、Node.jsの脆弱性に関するSingleIDへの影響

文書更新日:2026-10-22

SingleIDでは、お客様にご対応をお願いする必要がある場合を除き、個別の脆弱性に関する対応状況は通常公表しておりません。

このたび、広く利用されているPHPにおいて、認証なしで外部から任意のコードを実行される可能性がある、2件の重大な脆弱性が公表されました。国内では、IPAによる事前の注意喚起や高市総理による速やかな対応の呼びかけに加え、財務省がWebサイトの一部を一時停止するなど、社会的にも大きな影響が生じています。

こうした状況を踏まえ、お客様に安心してSingleIDをご利用いただけるよう、今回のPHP脆弱性によるSingleIDへの影響をお知らせします。併せて、同時期に公表されたOpenJDK、Apache Tomcat、Node.jsの脆弱性についても、深刻度の高いものを中心に、影響の有無と対応状況をご案内します。

!!! info
    * SingleIDの認証サービスは通常どおりご利用いただけます。
    * 現時点で、お客様側で実施いただく作業はありません。

## PHP

| 項目 | 内容 |
| :--- | :--- |
| 公表日 | 2026年10月21日 |
| 対象 | `CVE-2026-109618`、`CVE-2026-109619` |
| 深刻度 | Critical（CVSS 4.0: 9.2） |
| SingleIDへの影響 | PHPを使用していないため、影響はありません。 |

## OpenJDK

| 項目 | 内容 |
| :--- | :--- |
| 公表日 | 2026年10月21日 |
| 対象 | `CVE-2026-91403` |
| 深刻度 | Critical（CVSS 3.1: 9.1） |
| SingleIDへの影響 | 2026年10月22日に対策を完了しており、現在、SingleIDへの影響はありません。 |
| 対応履歴 | 2026年10月21日: クラウドRADIUSおよびプライベートPKIへの影響はありませんでした。アプリ連携機能の一部は影響を受ける可能性がありましたが、接続先を限定しているため、リスクは低いと評価しました。<br>2026年10月22日: サービスを停止せずに対策を完了しました。 |

## Apache Tomcat

| 項目 | 内容 |
| :--- | :--- |
| 公表日 | 2026年10月20日 |
| 対象 | `CVE-2026-101356`、`CVE-2026-101296`、`CVE-2026-93446`、`CVE-2026-88041`、`CVE-2026-74049`、`CVE-2026-74048`、`CVE-2026-103119`、`CVE-2026-80353` |
| 深刻度 | Important: `CVE-2026-101356`、`CVE-2026-101296`、`CVE-2026-103119`<br>Low: `CVE-2026-93446`、`CVE-2026-88041`、`CVE-2026-74049`、`CVE-2026-74048`、`CVE-2026-80353` |
| SingleIDへの影響 | Apache Tomcatを使用していないため、影響はありません。 |

## Node.js

| 項目 | 内容 |
| :--- | :--- |
| 公表日 | 2026年10月21日 |
| 対象 | `CVE-2026-105481`、`CVE-2026-105482`、`CVE-2026-105483`、`CVE-2026-106069`、`CVE-2026-106070`、`CVE-2026-106071`、`CVE-2026-106072`、`CVE-2026-106073`、`CVE-2026-106074`、`CVE-2026-111497` |
| 深刻度 | High: `CVE-2026-105482`、`CVE-2026-105483`、`CVE-2026-106069`、`CVE-2026-111497`<br>Medium: `CVE-2026-105481`、`CVE-2026-106072`、`CVE-2026-106074`<br>Low: `CVE-2026-106070`、`CVE-2026-106071`、`CVE-2026-106073` |
| SingleIDへの影響 | Node.jsを使用していないため、影響はありません。 |

## 参考情報

* [PHP セキュリティ情報](https://www.php.net/archive/2026.php){ target=_blank }
* [【事前準備のお願い】PHPのセキュリティリリース公開予定について（IPA）](https://www.ipa.go.jp/security/security-alert/2026/pre-alert20261021.html){ target=_blank }
* [高市総理「大至急、PHPパッチ適用を」（ITmedia NEWS）](https://www.itmedia.co.jp/news/article/2610/22/2000002483/){ target=_blank }
* [PHPの脆弱性対応に伴うWebサイト・一部コンテンツの一時停止のお知らせ（財務省）](https://www.mof.go.jp/public_relations/ohter/20261021231537.html){ target=_blank }
* [OpenJDK Vulnerability Advisory: 2026/10/21](https://openjdk.org/groups/vulnerability/advisories/2026-10-21){ target=_blank }
* [Apache Tomcat 9 セキュリティ情報](https://tomcat.apache.org/security-9.html#Fixed_in_Apache_Tomcat_9.0.123){ target=_blank }
* [Apache Tomcat 11 セキュリティ情報](https://tomcat.apache.org/security-11.html#Fixed_in_Apache_Tomcat_11.0.27){ target=_blank }
* [Node.js セキュリティリリース](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases){ target=_blank }
