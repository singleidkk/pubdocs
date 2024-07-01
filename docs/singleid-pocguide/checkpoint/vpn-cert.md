# リモートアクセスVPN-クライアント証明書認証
## 目的
* SingleIDのユーザで、Check Point Quantum SparkへVPNを使ってリモートアクセスします。
* 接続する際の認証方式は、クライアント証明書認証です。
* SingleIDの標準RADIUSサーバを利用します。

## 設定方法
### SingleIDのユーザの作成
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. **登録**ボタンをクリックします。**ユーザ登録**画面がポップアップします。
3. **ユーザ登録**画面の**基本情報**を入力します。

### SingleIDのクライアント証明書の発行
1. **SingleID 管理者ポータル＞ユーザ**画面へ移動します。
2. クライアント証明書を発行したいユーザの行にある**チェックボックス**を選択します。
3. **選択実行**ボタンの▼をクリックし、プルダウンメニューの**証明書の発行**を選択します。
4. **証明書発行**画面がポップアップします。
5. **プロファイル**（例：デフォルト）および**配布形式**（例：一般）を選択します。
6. **発行**ボタンをクリックして、クライアント証明書を発行します。ユーザのメールアドレス宛に発行されたクライアント証明書のダウンロードリンクが送信されます。

### SingleIDの中間CA証明書のダウンロード
1. **SingleID 管理者ポータル＞認証＞証明書**画面の**基本情報**タブへ移動します。
2. **中間CA**ブロックの中間CAのタイトル文字の隣の:fontawesome-solid-cloud-arrow-down:をクリックします。**ダウンロード**画面がポップアップします。
3. **ダウンロード**ボタンをクリックして、SingleIDの中間CA証明書をダウンロードします。

!!! warning
    誤って、ルートCA証明書をダウンロードしないようにご注意ください。ルートCA証明書のダウンロードは必要ありません。

### Check Point Quantum SparkのトラストCAへSingleIDの中間CA証明書を追加
1. **Check Point Quantum Spark GUI＞VPN＞トラストCA**画面へ移動します。
2. **追加**ボタンをクリックします。**トラストCAの追加**画面がポップアップします。
3. 以下を設定し、**適用**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **トラストCAファイル** | ダウンロードした**SingleIDの中間CA証明書ファイル**を選択 |
    | **CA名** | 任意のCA名（例えば、「SingleID User CA」） |

    !!! warning
        CA名に、2バイト文字は使用できません。
    
    !!! danger
        **SingleIDのルートCA証明書をCheck Point Quantum Sparkへインポートしないようにしてください。意図しないVPN接続の認証が成功してしまいます。**
        
        リモートアクセスクライアントからCheck Point Quantum SparkへのVPN接続時、SingleIDのルートCA証明書がCheck Point Quantum Sparkにインポートされていると、SingleIDのルートCAを発行元とするどんな中間CAが発行したクライアント証明書であっても、証明書のパスの検証に成功してしまいます。

## 動作確認方法
### リモートアクセスVPNの認証（クライアント証明書認証）

#### クライアント証明書のダウンロード
1. **SingleIDシステム管理**から届いたメールを開きます。
2. **ダウンロード**リンクをクリックします。P12形式のクライアント証明書がダウンロードされます。

    [![Screenshot](/images/2021-09-02_19-10-04-1024x489-1.png)](/images/2021-09-02_19-10-04-1024x489-1.png)

!!! info
    もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードを スマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、 スマートフォンやタブレット にもクライアント証明書をダウンロードできます。

#### リモートアクセスクライアントのインストール
Check Point Quantum Sparkのリモートアクセスクライアントである、 Check Point Remote Access VPN Clients をインストールしていない場合には、以下よりダウンロードしてインストールします。

[ダウンロード](https://supportcenter.checkpoint.com/supportcenter/portal?version=&amp;os=&amp;productTab=downloads&amp;product=175&amp;eventSubmit_doShowproductpage=){ target=_blank .md-button .md-button--primary }

#### 接続先の設定

1. Check Point Remote Access VPN Clientsを起動して、新規サイトを作成します。サイトウィザードが表示されたら、**Next**ボタンをクリックします。

    [![Screenshot](/images/image-24.png)](/images/image-24.png)

2. **Server address or Name**に、Check Point Quantum SparkのWANのIPアドレスを入力し、**Next**ボタンをクリックします。

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
    | **user name** | **Browse**をクリックし、ダウンロードしたクライアント証明書を選択します。 |
    | **password** | クライアント証明書のパスワードを入力します。クライアント証明書のパスワードは、ユーザ名と同一です。（例：ユーザ名が、user1 の場合、証明書のパスワードは user1 です。） |

    [![Screenshot](/images/image-cpvpnclient-importcert.png)](/images/image-cpvpnclient-importcert.png)

4. クライアント証明書がインポートされました。**OK**ボタンをクリックします。

    [![Screenshot](/images/image-cpvpnclient-importcert-ok.png)](/images/image-cpvpnclient-importcert-ok.png)

5. **Connect**ボタンをクリックし、Check Point Quantum SparkへVPN接続を試み、接続が成功することを確認します。

    [![Screenshot](/images/image-cpvpnclient-cert-connect.png)](/images/image-cpvpnclient-cert-connect.png)

6. **Details**ボタンをクリックすると、詳細が表示され、接続が成功したことが確認できます。

    [![Screenshot](/images/image-32.png)](/images/image-32.png)

