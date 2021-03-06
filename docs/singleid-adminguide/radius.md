**管理者ポータル画面＞認証＞RADIUS**画面では、以下のRADIUSに関する操作を行うことができます。

* クライアントIPの登録/編集/削除
* サービスアカウントの登録/編集/削除

## サポートする認証方式について
以下の認証方式をサポートしています。

* PAP
* CHAP
* MSCHAPv2
* EAP-TLS
* EAP-TTLS-PAP
* PEAP(MSCHAPv2)

ワンタイムパスワード認証を行う場合には、以下の認証方式のみサポートします。

* PAP
* EAP-TLS-PAP

## 基本情報
**管理者ポータル画面＞認証＞RADIUS＞基本情報**画面では、RADIUSクライアントを設定するために必要な情報が記載されています。

## ベンダー設定/簡易設定
**管理者ポータル画面＞認証＞RADIUS＞ベンダー設定/簡易設定**画面では、RADIUSサーバの設定を行います。RADIUSをサポートする無線LANやファイアウォールと連携をするために、RADIUSの属性の知識は必要ありません。RADIUS属性の設定を意識することなく、RADIUSサーバの設定を簡単に行うことができます。

### RADIUSのサイトの登録
1. RADIUSクライアントとなる機器の**ベンダー名**タブを選択します。該当するベンダー名がない場合には、**簡易設定**タブをクリックします。
2. **登録**ボタンをクリックすると設定画面がポップアップします。
3. **基本情報**タブでは、以下の項目を設定します。
   
    * **サイト名**：設定は任意です。複数サイトを登録する場合には、どの機器のRADIUS設定かを識別するために名前を設定することをお勧めします。
    * **有効/無効**：サイトの有効/無効を設定します。無効の場合には、RADIUSクライアントとなる機器は、RADIUSサーバへ接続できませんので、RADIUSによる認証はできません。
    * **サーバ**：1～10までのサーバを選択します。サーバの番号とRADIUSサーバのポート番号のマッピングは、**管理者ポータル＞認証＞RADIUS＞基本情報**画面を参照してください。
    * **ワンタイムパスワード強制**：**VPNアクセスの認証**および**管理者アクセスの認証**について、ワンタイムパスワード認証を強制します。**無線アクセスの認証**については、ワンタイムパスワード認証は行われません。

        !!! warning
            
            1. 2要素認証（パスワード認証＋ワンタイムパスワード）は、PAPまたはEAP-TLS-PAP認証時のみ可能となります。その他のパスワード入力タイプの認証方式（CHAP、MSCHAP、PEAP）の場合には、ワンタイムパスワードは利用できません。

            2. 一般的に、無線LANアクセスの認証に、ワンタイムパスワードが利用されることは少ないことから、無線LANアクセスの認証時には、ワンタイムパスワードは無効となり、ワンタイムパスワードは利用できません。RADIUSクライアントとなる機器から送信されるCalled-Station-IDにSSIDが含まれている場合に、無線LANアクセスの認証要求と判断しています。

            3. ワンタイムパスワードトークンの設定を行っていないユーザは、ワンタイムパスワードは無効となり、ワンタイムパスワードは利用できません。

    * **シークレット**：RADIUSクライアントとRADIUSサーバで共通で使用する共有シークレットです。
    * **IP or ホスト名**：RADIUSクライアントのIPまたはホスト名です。

 1. **VPNアクセスの認証**/**無線アクセスの認証**/**管理者アクセスの認証**タブをクリックし、RADIUSクライアントとなる機器へのアクセスを許可する**ユーザ**/**グループ**を設定します。

!!! warning
    設定がシステムへ反映されるまでに、最大で15分かかります。

### RADIUSのサイトの編集
1. 変更したいサイトの列にある**編集**ボタンをクリックします。
2. **設定画面**がポップアップします。
3. 項目を変更して、**更新**ボタンをクリックして、変更を保存します。

!!! warning
    設定がシステムへ反映されるまでに、最大で15分かかります。

### RADIUSのサイトの削除
1. 削除したいサイトの列にある**削除**ボタンをクリックします。
2. **簡易設定削除**画面がポップアップします。
3. **削除**ボタンをクリックして、サイトを削除します。
!!! warning
    設定がシステムへ反映されるまでに、最大で15分かかります。

## 高度な設定
**管理者ポータル画面＞認証＞RADIUS＞高度な設定**画面では、RADIUSサーバのサイト、クライアント、ルールの設定を手動で設定します。特に、ルールの作成については、RADIUSの属性と値を指定する必要があるため、RADIUSに関する高度な知識が要求されます。
以下のような場合に使用してください。

* 簡易設定ではできない複雑なルールを定義したい
* 簡易設定ではサポートしていないベンダー属性を使用したルールを作成したい
* 同一のサーバ番号で、複数のサイトを作成したい