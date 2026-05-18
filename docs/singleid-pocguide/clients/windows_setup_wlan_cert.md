# Windows: クライアント証明書で無線LANアクセスの認証（接続時に確認）

この手順では、Windowsで無線LANへ接続し、接続時に表示されるRADIUSサーバ証明書の名前を確認して信頼します。

!!! info
    事前に、認証で使用するクライアント証明書をWindowsへインストールしてください。CAとRADIUSサーバのホスト名を事前に指定して検証する場合は、[CAとホスト名を指定して検証する場合](./windows_setup_wlan_cert2.md)を使用してください。

1. 無線LANアクセスポイントのSSIDを選択し、**接続**ボタンをクリックします。

    [![Screenshot](/images/image-34.png)](/images/image-34.png)

2. **証明書を使って接続する**をクリックします。

    [![Screenshot](/images/image-certwifilogin-1.png)](/images/image-certwifilogin-1.png)

3. 認証で使用するクライアント証明書を選択し、**OK**ボタンをクリックします。

    [![Screenshot](/images/image-certwifilogin-2.png)](/images/image-certwifilogin-2.png)

4. 接続先が正しいかどうかの確認を要求されます。**証明書の詳しい内容の表示**でRADIUSサーバのサーバ証明書の情報が表示されますので、接続先の確認を行います。接続先が正しければ、**接続**ボタンをクリックします。

    | **項目** | **内容** |
    | :--- | :--- |
    | **発行先** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。 |

    表示されたサーバ証明書の**発行先**が、上記のホスト名と一致していることを確認してください。**発行元**は変更される場合があるため、固定の値ではなく証明書チェーンが信頼できることを確認します。

    [![Screenshot](/images/image-48.png)](/images/image-48.png)

5. 接続成功したことを確認します。

    [![Screenshot](/images/image-39.png)](/images/image-39.png)
