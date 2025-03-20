# Alpine LinuxにCSE Connectorをインストールする手順
文書更新日:2025-03-20

## 事前準備
CSE Connectorのセットアップスクリプトを実行するために必要なパッケージをAlpine Linuxへインストールします。
### `bash`のインストール
以下のコマンドでインストールします。

``` title="Alpine Linuxで実行"
apk add bash
```

### `sudo`のインストール
`cat /etc/apk/repositories`コマンドを実行し、communityリポジトリが有効であることを確認します。コメントアウトされている（URLの先頭に#ついている）場合には、アンコメント（#を消去）します。以下のコマンドでインストールします。

``` title="Alpine Linuxで実行"
apk add sudo
```

## CSE Connectorのインストール
Alpine Linux用にセットアップスクリプトおよびスタートアップスクリプトを修正したCSE Connectorをダウンロードします。

1. ダウンロードします。

    ``` title="Alpine Linuxで実行"
    wget -O connector.tar.gz https://github.com/singleidkk/connector-alpine/archive/refs/tags/2.0.0.tar.gz
    ```

2. 解凍します。

    ``` title="Alpine Linuxで実行"
    tar xvfz connector.tar.gz
    ```

3. 解凍されたディレクトリへ移動します。

    ``` title="Alpine Linuxで実行"
    cd connector-alpine-2.0.0
    ```

4. **CSE管理UI＞Networs＞Connectors**画面へ移動します。登録済み**Connector**をクリックします。**Install**タブをクリックします。
5. 「Where will you install this Connector?」に、**Linux**を選択します。「Select an Installation Method」に、**Tarball Installer**を選択します。**Step3**に記載されたコマンドをコピーして、Alpine Linuxで実行します。

    ``` title="Alpine Linuxで実行 サンプル"
    echo 'command_center_url: https://net.banyanops.com
    api_key_secret: xxxxx
    connector_name: xxxxx' > connector-config.yaml
    ```

6. セットアップスクリプトを実行します。CSE Connectorがインストールされます。

    ``` title="Alpine Linuxで実行"
     ./setup-connector-alpine.sh
    ```

7. **CSE管理UI＞Networs＞Connectors**画面へ移動します。登録済み**Connector**をクリックします。**Status**が**Reporting**となっていることを確認します。

    [![Screenshot](/images/2025-03-17_16-34-07.png)](/images/2025-03-17_16-34-07.png)


