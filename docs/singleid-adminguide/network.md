**SingleID 管理者ポータル＞管理＞ネットワーク**画面では、以下のSingleID ネットワークサービスで利用するホスト（FQDN）を設定します。

* ダイナミックDNSサービス
* トンネルサービス（対応予定）

!!! warning
    SingleID ネットワークサービスは、**３ホスト（FQDN）まで無償**で登録し利用できます。４ホスト以上のご利用は、有償オプションとなります。**４ホスト以上をご利用になりたい場合には、販売店様へお問い合わせください。**

## ダイナミックDNSサービス
ダイナミックDNSホスト（FQDN）は、IPv4およびIPv6に対応しており、以下の仕様でDNSへ登録されます。

**IPの種類：IPv4アドレス**

* DNSレコードの種類：Aレコード
* DNSのTTL：30秒

**IPの種類：IPv6アドレス**

* DNSレコードの種類：AAAAレコード
* DNSのTTL：30秒

### ダイナミックDNSホスト（FQDN）の登録
`<ホスト名>.<DNSゾーン>`形式のダイナミックDNSホスト（FQDN）を作成します。

1. **登録**ボタンをクリックします。
2. **登録**画面がポップアップします。
3. **ホスト名**を入力します。
4. **DNSゾーン**を選択します。
5. **IPタイプ**を選択します。
6. **APIキー**が生成されます。このAPIキーは、後述のIPアドレス登録APIで使用します。

!!! warning
    IPタイプでIPv4を選択した場合には、IPv6のIPをDNSへ登録できません。同様に、IPタイプで、IPv6を選択した場合には、IPv4のIPをDNSへ登録できません。

### ダイナミックDNSホスト（FQDN）の削除

1. 削除したいダイナミックDNSホスト（FQDN）の列にある**削除**ボタンをクリックします。
2. **削除**画面がポップアップします。
3. **削除**ボタンをクリックし、ダイナミックDNSホスト（FQDN）を削除します。

!!! warning
    削除後、同じ名前でダイナミックDNSホスト（FQDN）を登録しても、削除前のAPIキーと同じにはなりません。

### ダイナミックDNSホスト（FQDN）にIPを関連付ける
ダイナミックDNSホスト（FQDN）にIPアドレスを関連付けるには、APIを使用します。以下は、CURLコマンドを利用してAPIを実行した場合の例です。

!!! warning
    ダイナミックDNSホスト（FQDN）にIPアドレスを関連付けるAPIリクエストを定期的に実行する場合は、DNSレコードのキャッシュ時間（TTL30秒）よりも短くならない間隔で実行してください。

#### 方法１　APIリクエストの送信元のグローバルIPアドレスを関連付ける
1. 以下のCURLコマンドを社内LAN内にあるコンピュータで実行します。

    | **パラメータ** | **内容** |
    | :--- | :--- |
    | **APIキー** | **顧客管理者ポータル＞管理＞ネットワーク**画面で登録したダイナミックDNSホストに関連付いたAPIキーです。 |
    | **エンドポイントのホスト名** | **本番環境**: tun-00-0001.service.singleid.jp <br>**PoC環境**: tun-02-0001.poc.singleid.jp |

    ```
    curl -X 'POST' \
        'https://<エンドポイントのホスト名>/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d ''
    ```

    !!! note
        IPv4とIPv6の両方のアドレスをもったインターネット回線の場合、curlコマンドでIPv4またはIPv6を明示して実行する必要があります。

    **例 IPv4のIPをDDNSホストに登録したい場合**
    ```
    curl -4 -X 'POST' \
        'https://<エンドポイントのホスト名>/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d ''
    ```

    **例 IPv6のIPをDDNSホストに登録したい場合**
    ```
    curl -6 -X 'POST' \
        'https://<エンドポイントのホスト名>/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d ''
    ```

2. 会社のインターネット接続のグローバルIPアドレスが、ダイナミックDNSホスト（FQDN）のDNSレコードに設定されます。

#### 方法２　関連付けたいIPアドレスを明示的に指定する
1. 以下のCURLコマンドをコンピュータで実行します。

    | **パラメータ** | **内容** |
    | :--- | :--- |
    | **APIキー** | **顧客管理者ポータル＞管理＞ネットワーク**画面で登録したダイナミックDNSホストに関連付いたAPIキーです。 |
    | **IPアドレス** | 登録したいグローバルIPアドレスです。 |
    | **エンドポイントのホスト名** | **本番環境**: tun-00-0001.service.singleid.jp <br>**PoC環境**: tun-02-0001.poc.singleid.jp |

    ```
    curl -X 'POST' \
        'https://<エンドポイントのホスト名>/box/forwarding/ddns' \
        -H 'accept: application/json' \
        -H 'Authorization: token <APIキー>' \
        -H 'Content-Type: application/json' \
        -d '{"global_ip": "<IPアドレス>"}'
    ```

2. 指定したグローバルIPアドレスが、ダイナミックDNSホスト（FQDN）のDNSレコードに設定されます。

#### 方法３　関連付けたいIPアドレスを明示的に指定する（DynDNS互換API）
1. 以下のCURLコマンドをコンピュータで実行します。

    | **パラメータ** | **内容** |
    | :--- | :--- |
    | **ホスト名** | **顧客管理者ポータル＞管理＞ネットワーク**画面で登録したダイナミックDNSホストのホスト名です。 |
    | **FQDN** | **顧客管理者ポータル＞管理＞ネットワーク**画面で登録したホスト名に関連付いたDNSゾーンを含むダイナミックDNSホスト（FQDN）です。 |
    | **APIキー** | **顧客管理者ポータル＞管理＞ネットワーク**画面で登録したダイナミックDNSホストに関連付いたAPIキーです。 |
    | **IPアドレス** | 登録したいグローバルIPアドレスです。 |
    | **エンドポイントのホスト名** | **本番環境**: tun-00-0001.service.singleid.jp <br>**PoC環境**: tun-02-0001.poc.singleid.jp |

    ```
    curl -X 'GET' \
    -u <ホスト名>:<APIキー> \
    'https://<エンドポイントのホスト名>/nic/update?hostname=<FQDN>&myip=<IPアドレス>'
    ```

2. 指定したグローバルIPアドレスが、ダイナミックDNSホスト（FQDN）のDNSレコードに設定されます。
