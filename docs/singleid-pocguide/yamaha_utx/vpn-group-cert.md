# リモートアクセスVPN-クライアント証明書認証
## 目的
SingleIDのユーザで、YAMAHA UTXへVPNを使ってリモートアクセスします。
接続する際の認証方式は、クライアント証明書認証です。

## 環境
### ユーザの情報
| **ユーザ名** | **姓（英字）** | **名（英字）** | **メールアドレス** |
| :--- | :--- | :--- | :--- |
| user1 | user1 | user1 | user1@poc.singleid.jp |

!!! info
    メールアドレス: 受信可能なメールアドレスを指定してください。

## 設定方法
### SingleIDの設定
#### 管理者ポータルへログイン
1. SingleIDの[**管理者ポータル**](https://www.singleid.jp/product-login/){target=_blank}へログインします。

#### ユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。作成するユーザは、[ユーザの情報](#ユーザの情報)を参照します。 

#### クライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。クライアント証明書を発行するユーザは、[ユーザの情報](#ユーザの情報)を参照します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**を選択します。（例：デフォルト）
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

#### SingleIDの中間CA証明書のダウンロード
1. **SingleID 管理者ポータル＞認証＞証明書**画面の**基本情報**タブへ移動します。
2. **中間CA**ブロックの中間CAのタイトル文字の隣の:fontawesome-solid-cloud-arrow-down:をクリックします。**ダウンロード**画面がポップアップします。
3. **ダウンロード**ボタンをクリックして、SingleIDの中間CA証明書をダウンロードします。

!!! warning
    誤って、ルートCA証明書をダウンロードしないようにご注意ください。ルートCA証明書のダウンロードは必要ありません。

### YAMAHA UTXの設定
#### ローカル管理者でログイン
1. **YAMAHA UTX GUI** https://YAMAHA UTXのIP:4434/ へアクセスします。
2. ローカル管理者のユーザ名、パスワードを入力し、**ログイン**をクリックします。

#### トラストCAへSingleIDの中間CA証明書を追加
1. **YAMAHA UTX GUI＞VPN＞トラストCA**画面へ移動します。
2. **追加**ボタンをクリックします。**トラストCAの追加**画面がポップアップします。
3. 以下を設定し、**適用**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **トラストCAファイル** | ダウンロードした**SingleIDの中間CA証明書ファイル**を選択 |
    | **CA名** | 任意のCA名（例えば、「SingleID User CA」） |

    !!! warning
        CA名に、２バイト文字は使用できません。
    
    !!! danger
        **SingleIDのルートCA証明書をYAMAHA UTXへインポートしないようにしてください。意図しないVPN接続の認証が成功してしまいます。**
        
        リモートアクセスクライアントからYAMAHA UTXへのVPN接続時、SingleIDのルートCA証明書がYAMAHA UTXにインポートされていると、SingleIDのルートCAを発行元とするどんな中間CAが発行したクライアント証明書であっても、証明書のパスの検証に成功してしまいます。

## 動作確認方法
### リモートアクセスVPNの認証（クライアント証明書認証）

#### クライアント証明書のダウンロード
1. **SingleIDシステム管理**から届いたメールを開きます。
2. **ダウンロード**リンクをクリックします。P12形式のクライアント証明書がダウンロードされます。

    [![Screenshot](/images/2021-09-02_19-10-04-1024x489-1.png)](/images/2021-09-02_19-10-04-1024x489-1.png)

!!! info
    もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードを スマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、 スマートフォンやタブレット にもクライアント証明書をダウンロードできます。

#### リモートアクセスクライアントのインストール
YAMAHA UTXのリモートアクセスクライアントである、 Check Point Remote Access VPN Clients をインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://supportcenter.checkpoint.com/supportcenter/portal?version=&amp;os=&amp;productTab=downloads&amp;product=175&amp;eventSubmit_doShowproductpage=){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. Check Point Remote Access VPN Clientsを起動して、新規サイトを作成します。サイトウィザードが表示されたら、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-24.png)](/images/image-24.png)

2. **Server address or Name**に、YAMAHA UTXのWANのIPアドレスを入力し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-25.png)](/images/image-25.png)

3. **VPN Client (Default)**が選択されていることを確認し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-26.png)](/images/image-26.png)

4. **Certificate**を選択し、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-cpvpnclient-cert.png)](/images/image-cpvpnclient-cert.png)

5. **CAPI**を選択し、**Next**ボタンをクリックします。

    !!! info
        どちらを選択しても、クライアント証明書を利用した認証を行うことができます。**CAPIを選択した場合**には、一度、クライアント証明書をPCへインポートすれば、VPN接続時には自動ログインできます。**PKCS #12を選択した場合**には、VPN接続のたびに、クライアント証明書のパスワードが尋ねられます。

    [![Screenshot](/images/image-cpvpnclient-capi.png)](/images/image-cpvpnclient-capi.png)

6. **Site created successfully**が表示されたら、サイトが無事作成されました。**Finish**ボタンをクリックします。

    [![Screenshot](/images/image-29.png)](/images/image-29.png)

#### VPN接続

1. **Connect**ボタンをクリックします。 

    [![Screenshot](/images/image-30.png)](/images/image-30.png)

2. **Site**に、作成した接続先を選択します。

3. 以下を設定し、**Import**ボタンをクリックします。
    
    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **user name** | **Browse**をクリックし、ダウンロードした**SingleIDのユーザ**[（参照）](#ユーザの情報)のクライアント証明書を選択します。 |
    | **password** | クライアント証明書のパスワードを入力します。クライアント証明書のパスワードは、**SingleIDのユーザ名**[（参照）](#ユーザの情報)と同一です。 |

    [![Screenshot](/images/image-cpvpnclient-importcert.png)](/images/image-cpvpnclient-importcert.png)

4. クライアント証明書がインポートされました。**OK**ボタンをクリックします。

    [![Screenshot](/images/image-cpvpnclient-importcert-ok.png)](/images/image-cpvpnclient-importcert-ok.png)

5. **Connect**ボタンをクリックし、YAMAHA UTXへVPN接続を試み、接続が成功することを確認します。

    [![Screenshot](/images/image-cpvpnclient-cert-connect.png)](/images/image-cpvpnclient-cert-connect.png)

6. **Details**ボタンをクリックすると、詳細が表示され、接続が成功したことが確認できます。

    [![Screenshot](/images/image-32.png)](/images/image-32.png)

