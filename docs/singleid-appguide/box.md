# Boxのアプリ連携
## シングルサインオン
* シングルサインオンが利用可能なプランは、Box Businessプラン以上です。詳しくは、[こちら](https://www.box.com/ja-jp/pricing){ target=_blank }を参照して下さい。
* 本資料は、[Setting Up Single Sign-On (SSO) for Your Enterprise](https://support.box.com/hc/en-us/articles/360043696514-Setting-Up-Single-Sign-On-SSO-for-your-Enterprise){ target=_blank }を参考にしています。
* Boxのシングルサインオン設定は、お客様自身で設定することはできません。Boxのテクニカルサポートによって作業を行います。**日本語でのサポートをご希望の場合には、ご契約しているBoxのサポート窓口までご相談することをお勧めします。**
* Boxが提供する公式アプリは、証明書認証に対応していない場合があります。SingleIDの証明書認証を必須とする場合には、ご注意ください。

### SingleIDのアプリ登録およびIdPエンドポイントメタデータのダウンロード
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. **カタログ表示**ボタンをクリックします。
    
    [![Screenshot](/images/2022-08-16_3-53-18.png)](/images/2022-08-16_3-53-18.png)

3. アプリカタログが表示されるので、Boxの**登録**ボタンをクリックします。
    
    [![Screenshot](/images/2022-11-01_1-57-40.png)](/images/2022-11-01_1-57-40.png)

4. **アプリ登録**画面がポップアップします。**情報**タブの**名前**に、アプリを識別できるような名前を、半角小文字の**英数字**、**-（ハイフン）**、**_（アンダースコア）**で設定します。
5. **シングルサインオン**タブに移動します。
    
    [![Screenshot](/images/2022-11-01_1-59-11.png)](/images/2022-11-01_1-59-11.png)

6. **SSO 有効/無効**を**有効**に設定します。
7. **IdP エンドポイントメタデータ**の**ダウンロード**ボタンをクリックし、メタデータを取得します。このメタデータは、Boxの設定を行うときに必要となります。
8. 以下の内容で設定し、**登録**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **NameIDフォーマット** | email |
    | **パスワード認証** | パスワード認証の有効/無効の設定です。 |
    | **証明書認証** | 証明書認証の有効/無効の設定です。 |
    
    !!! info
        * **パスワード認証：有効、証明書認証：有効の場合**、ログイン時に、**パスワード認証または証明書認証の認証方式の選択が可能**です。証明書がデバイスにインストールされていない場合には、自動的に、パスワード認証方式が選択されます。
        * **パスワード認証：無効、証明書認証：有効の場合**、ログイン時に、**証明書認証のみが可能**です。証明書がインストールされていないデバイスからのログインはできません。

    [![Screenshot](/images/2022-11-01_2-02-37.png)](/images/2022-11-01_2-02-37.png)

### BoxのSAML認証設定
!!! warning
    BoxのSAML認証設定は、Boxのテクニカルサポートによって作業を行います。ここでの説明は、英語でのサポートとなる恐れがあります。日本語でのサポートをご希望の場合には、**ご契約しているBoxのサポート窓口までご相談することをお勧めします。**

[Box SSO Setup Support Form](https://support.box.com/hc/en-us/requests/new?ticket_form_id=360002612594){ target=_blank }へ以下のように記入してサポートリクエストを送信します。

| **設問** | **記入内容** |
| :-- | :-- |
| **What can we help you with?** | **SSO Setup Support (including new setups and IdP changes)**を選択 |
| **Box Subdomain** | ご契約のBoxのサブドメインを入力します。（例：xxxxx.app.box.com） |
| **What is the attribute for the user's email?** | email |
| **What is the attribute for groups?** | member |
| **What is the attribute for the user's first name?** | firstName |
| **What is the attribute for the user's last name?** | lastName |
| **Who is your Identity Provider?** | **Other with Metadata**を選択 |
| **Anything else we should know?** | IdP metadata file is attached. <br>（SAMLのJITプロビジョニングを有効化したい場合には、**Enterprise SSO on the fly registrationの有効化**をするようにリクエストしてください。）|
| **Attachments (maximum attachment size 20MB)** | [SingleIDのアプリ登録およびIdP エンドポイントメタデータのダウンロード](#singleidのアプリ登録およびidp-エンドポイントメタデータのダウンロード)の手順7でダウンロードしたファイルを添付します。 |

SSOのセットアップ完了の連絡がありましたら、以下の手順を実施します。

1. Boxへ管理者権限を持つユーザでログインします。
2. 左サイドメニューの一番下にある、**管理コンソール**を選択して管理画面へ移動します。

    [![Screenshot](/images/2022-11-01_2-13-20.png)](/images/2022-11-01_2-13-20.png)

3. 左サイドメニューの**Enterprise設定**をクリックします。
    
    [![Screenshot](/images/2022-11-01_2-17-14.png)](/images/2022-11-01_2-17-14.png)

4. ユーザ設定タブへ移動します。ユーザ設定画面が表示されます。

    [![Screenshot](/images/2022-11-01_2-20-29.png)](/images/2022-11-01_2-20-29.png)

5. **すべてのユーザをシングルサインオン（SSO）を有効化**セクションの**SSO有効モード**を**有効**にします。

    [![Screenshot](/images/2022-11-01_18-41-37.png)](/images/2022-11-01_18-41-37.png)

## プロビジョニング
Boxは、SAMLのJITプロビジョニングに対応しています。設定は、Boxのテクニカルサポートによって作業を行います。有効化したい場合には、Boxサポートへ**Enterprise SSO on the fly registrationの有効化**をリクエストしてください。

JITプロビジョニングを有効化してない場合には、SingleIDおよびBoxの両方に存在するユーザのみが、シングルサインオン可能です。**SingleIDのメールアドレス**と**Boxのデフォルトのメールアドレス**が一致する必要があります。

## アプリにユーザ追加
1. **SingleID 管理者ポータル＞アプリ連携＞アプリ一覧**画面へ移動します。
2. 登録したアプリの列にある**チェックボックス**を選択します。
3. **ユーザ追加**ボタンをクリックします。
    
    [![Screenshot](/images/image-4.png)](/images/image-4.png)

4. **ユーザ追加**画面がポップアップします。アプリへ追加したい**ユーザ名**を選択し、**登録**ボタンをクリックして、アプリへユーザを追加します。
    
    [![Screenshot](/images/image-5.png)](/images/image-5.png)

## 動作確認
1. [Boxのログイン](https://account.box.com/login){ target=_blank }画面へアクセスします。
2. **メールアドレス**を入力し、**次へ**ボタンをクリックします。

    [![Screenshot](/images/2022-11-02_10-43-41.png)](/images/2022-11-02_10-43-41.png)

3. **SSOでサインイン**のリンクが追加されていることを確認します。

    [![Screenshot](/images/2022-11-02_10-45-32.png)](/images/2022-11-02_10-45-32.png)
    
4. **SSOでサインイン**リンクをクリックします。
5. SingleIDのログイン画面が表示されます。すでに、SingleIDへログイン中であれば、ログイン画面は表示されず、Boxへログインできます。
    
    [![Screenshot](/images/image-7-1024x462.png)](/images/image-7-1024x462.png)

## ダイレクトログインの無効化
!!! danger
    以下は、ダイレクトログインを無効化する方法です。**必ずSAML認証により管理者権限でログインできることを確認できてから、実施**するようにしてください。SAML認証ができない状態で、ダイレクトログインを無効化するとBoxへログインできなくなります。

1. Boxへ管理者権限を持つユーザでログインします。
2. **管理コンソール＞Enterprise設定＞ユーザ設定**画面へ移動します。
3. **すべてのユーザをシングルサインオン（SSO）を有効化**セクションの**SSO必須モード**を**有効**にします。**SSO必須モードを有効化**画面がポップアップします。

    [![Screenshot](/images/2022-11-02_10-57-46.png)](/images/2022-11-02_10-57-46.png)

4. **内容を必ず確認**し、同意したうえで、**すべてのユーザに対して有効化**ボタンをクリックします。

    [![Screenshot](/images/2022-11-02_11-00-12.png)](/images/2022-11-02_11-00-12.png)