# CSE Connectorを設定する手順

## 手順
1. SonicWall Cloud Secure EdgeのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. **CSE管理UI＞Networs＞Connectors**画面へ移動します。
3. **Create Connector**ボタンをクリックします。**Configure**画面が表示されます。
4. 以下の内容を設定し、**Continue**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Connector Name** | CSE Connecterをインストールするネットワークを識別する名前です。設定します。 |
    | **API Key** | **Select API Key**ボタンをクリックし、新規にAPIキーを生成するか、既存のAPIキーを選択します。 |
    | **What network would you like to make available?** | CSE Connecterをインストールするネットワークのネットワーク範囲をCIDRフォーマットで設定します。 デフォルトの**RFC 1918 Addresses**を選択した場合は、全てのプライベートIPアドレス範囲が定義されます。 |
    | **What domains exist on the network provided above? (optional)** | LAN用のローカルDNSゾーンがある場合にドメイン名を設定します。（例: example.local） |

    [![Screenshot](/images/2025-03-20_18-49-52.png)](/images/2025-03-20_18-49-52.png)

5. **Install**画面が表示されます。**Save & Exit**ボタンをクリックします。

