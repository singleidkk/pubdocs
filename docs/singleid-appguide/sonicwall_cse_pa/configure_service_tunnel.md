# Service Tunnelを設定する手順

## 手順
1. SonicWall Cloud Secure EdgeのWeb管理コンソールへ管理者権限を持つユーザでログインします。
2. **CSE管理UI＞Private Access＞Service Tunnels**画面へ移動します。
3. **Add Service Tunnel**ボタンをクリックします。設定の編集画面が表示されます。
4. 以下の内容を設定し、**SAVE**ボタンをクリックします。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **Name this Service Tunnel** | ユーザ側のCSE Appクライアントに表示される名前です。 |
    | **Network Settings** | **Add Network**ボタンをクリックし、[CSE Connectorを設定する手順](./configure_cse_connector.md)で作成したCSE Connectorを選択します。 |
    | **Attach a Policy** | **Attach a Policy**ボタンをクリックし、適用したいポリシーを選択します。（例: AllUsersAndDevices-Tunnel 全てのユーザ、すべてのデバイス） |
    | **Connect on Login** | CSE AppクライアントからCSEへログインしたときに、ここで設定したネットワークへアクセスするようにしたい場合には、有効にします。 |

    [![Screenshot](/images/2025-03-20_19-46-05.png)](/images/2025-03-20_19-46-05.png)
    [![Screenshot](/images/2025-03-20_19-47-58.png)](/images/2025-03-20_19-47-58.png)
    [![Screenshot](/images/2025-03-20_19-51-45.png)](/images/2025-03-20_19-51-45.png)
