**SingleID 管理者ポータル＞管理＞ネットワーク**画面では、以下のSingleID ネットワークサービスで利用するホスト（FQDN）を設定を行います。

* ダイナミックDNSサービス
* トンネルサービス（対応予定）

!!! warning
    SingleID ネットワークサービスは、３ホスト（FQDN）まで無償で登録し利用できます。４ホスト以上のご利用は、有償オプションとなります。４ホスト以上をご利用になりたい場合には、販売店様へお問い合わせください。

## ダイナミックDNSサービス

ダイナミックDNSホスト（FQDN）は、以下の仕様でDNSへ登録されます。

* DNSレコードの種類：Aレコード
* IPの種類：IPv4アドレス
* DNSのTTL：30秒

### ダイナミックDNSホスト（FQDN）の登録
`<ホスト名>.<DNSゾーン>`形式のダイナミックDNSホスト（FQDN）を作成します。

1. **登録**ボタンをクリックします。
2. **登録**画面がポップアップします。
3. **ホスト名**を入力します。
4. **DNSゾーン**を選択します。
5. **APIキー**が生成されます。

### ダイナミックDNSホスト（FQDN）の削除

1. 削除したいダイナミックDNSホスト（FQDN）の列にある**削除**ボタンをクリックします。
2. **削除**画面がポップアップします。
3. **削除**ボタンをクリックし、ダイナミックDNSホスト（FQDN）を削除します。

!!! warning
    削除後、同じ名前でダイナミックDNSホスト（FQDN）を登録しても、削除前のAPIキーと同じになりません。

### ダイナミックDNSホスト（FQDN）にIPを関連付ける
ダイナミックDNSホスト（FQDN）にIPアドレスを関連付けるには、APIを使用します。以下は、CURLコマンドを利用してAPIを実行した場合の例です。

#### 方法１　APIリクエストの送信元のグローバルIPアドレスを関連付ける

1. 以下のCURLコマンドの`<APIキー>`の部分に生成されたAPIキーの文字列を指定します。

    ```
    curl -X 'POST' \
        'https://tun-00-0001.service.singleid.jp/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d ''
    ```

2. このコマンドを社内LAN内にあるコンピュータで実行します。
3. 会社のインターネット接続のグローバルIPアドレスが、ダイナミックDNSホスト（FQDN）のDNSのＡレコードに設定されます。

#### 方法２　関連付けたいIPアドレスを明示的に指定する

1. 以下のCURLコマンドの`<APIキー>`の部分に生成されたAPIキーの文字列、`<IPアドレス>`の部分に関連付けたいグローバルIPアドレスを指定します。

    ```
    curl -X 'POST' \
        'https://tun-00-0001.service.singleid.jp/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d '{"global_ip": "<IPアドレス>"}'
    ```

2. このコマンドをコンピュータで実行します。
3. 指定したグローバルIPアドレスが、ダイナミックDNSホスト（FQDN）のDNSのＡレコードに設定されます。

