# FortiGateリモートアクセスVPN RadSec検証メモ
文書更新日:2026-05-26

!!! warning
    本書は、FortiGateのリモートアクセスVPNでSingleIDのクラウドRADIUSをRadSecで利用した検証メモです。正式な設定ガイドとして網羅的に検証したものではありません。

    FortiGate v7.6.6で動作確認した内容に基づいています。利用する場合は、事前に検証環境で動作確認してください。

## 目的
FortiGateのリモートアクセスVPN認証で、SingleIDのクラウドRADIUSとの通信をUDPからRadSecへ変更するための検証手順を整理します。

本書では、既存のFortiGateリモートアクセスVPN設定を作成した後、RADIUSサーバ設定だけをRadSec向けに変更します。

## 前提
以下の設定ガイドに従って、拡張RADIUSサーバを利用したリモートアクセスVPN接続が成功していることを前提にします。

* [リモートアクセスVPN-2要素認証（パスワード認証＋クライアント証明書認証）（IKEv2自動トランスポートモード）](vpn-group-password-cert-ikev2-adv.md)

また、以下の条件を満たしていることを確認します。

* SingleIDの拡張RADIUSサーバを利用していること
* FortiGateに、SingleIDで発行したサーバ証明書をインポート済みであること
* FortiGateからSingleIDのクラウドRADIUSのRadSecポートへ通信できること
* FortiGate側のRADIUSサーバ設定名を確認していること（例：`SingleID_RADIUS`）

!!! info
    この検証では、FortiGateにインポートしたSingleIDのサーバ証明書を、RadSec接続時のクライアント証明書として使用します。SingleIDで発行するサーバ証明書は、サーバ認証だけでなくクライアント認証にも利用できる証明書です。

## 設定手順

### 1. SingleIDの拡張RADIUSサーバをRadSecへ変更
**SingleID 管理者ポータル＞認証＞RADIUS＞基本設定**画面で、検証対象の拡張RADIUSサーバの**使用するプロトコル**を、UDPからRadSecへ変更します。

!!! info
    RadSecを利用する場合でも、既存のRADIUSサイト設定、許可ユーザ、許可グループ、NAS-Identifierなどの設定は、基本的にそのまま利用します。

### 2. FortiGateのRADIUSサーバ設定を変更
FortiGate CLIで、既存のRADIUSサーバ設定をRadSec向けに変更します。

1. 既存のRADIUSサーバ設定を確認します。

    ``` title="FortiGate CLI"
    show user radius
    ```

2. セカンダリサーバが設定されている場合は、削除します。

    ``` title="FortiGate CLI"
    config user radius
        edit "<RADIUSサーバ設定名>"
            unset secondary-server
            unset secondary-secret
        next
    end
    ```

    !!! info
        セカンダリサーバを設定していない場合、この手順は不要です。

3. プライマリサーバをRadSec向けに変更します。

    | **設定項目** | **設定内容** |
    | :--- | :--- |
    | **RADIUSサーバ設定名** | 既存のFortiGate RADIUSサーバ設定名です。（例：`SingleID_RADIUS`） |
    | **RADIUSサーバのホスト名** | **SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**ホスト名**です。（例：`radius-02-0001.poc.singleid.jp`） |
    | **RADIUS認証ポート番号** | SingleIDの拡張RADIUSサーバに割り当てられたRADIUSポート番号です。**SingleID 管理者ポータル＞認証＞RADIUS**画面の**基本情報**タブの**拡張RADIUSサーバ＞RADIUSポート番号**で確認できます。（例：`20000`） |
    | **クライアント証明書** | FortiGateにインポート済みのSingleIDサーバ証明書名です。証明書名は、証明書発行時に指定したIPアドレスまたはFQDNになっています。（例：`vpn.example.com`） |

    ``` title="FortiGate CLI"
    config user radius
        edit "<RADIUSサーバ設定名>"
            set server "<RADIUSサーバのホスト名>"
            set radius-port <RADIUS認証ポート番号>
            set transport-protocol tls
            set client-cert "<クライアント証明書>"
        next
    end
    ```

### 3. 設定例
以下は検証時の設定例です。

``` title="FortiGate CLI"
config user radius
    edit "SingleID_RADIUS"
        set server "radius-02-0001.poc.singleid.jp"
        set secret ENC xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
        set radius-port 20000
        set require-message-authenticator disable
        set transport-protocol tls
        set client-cert "vpn.example.com"
    next
end
```

!!! warning
    `set secret ENC ...` はFortiGate上で暗号化表示された共有シークレットです。実際のシークレット値をドキュメントやチケットに貼り付けないでください。

## 動作確認
FortiClientから、既存のリモートアクセスVPN設定で接続します。

接続できることを確認したら、以下を確認します。

* FortiClientでVPN接続が成功すること
* SingleIDのRADIUS認証ログに認証結果が記録されること
* FortiGateのRADIUSサーバ設定が、`transport-protocol tls` になっていること
* FortiGateから指定したRADIUSサーバのホスト名を名前解決できること

設定確認には、以下のコマンドを利用できます。

``` title="FortiGate CLI"
show user radius
```

## 補足

### RADIUSサーバにはFQDNを指定します
RadSecでは、FortiGateがSingleIDのクラウドRADIUSのサーバ証明書を検証します。

FortiGateのRadSecでは、`server-identity-check` がデフォルトで有効です。Fortinet公式ドキュメントでは、RADIUSサーバのドメイン名またはIPアドレスをサーバ証明書と照合する機能として説明されています。

そのため、`set server` にはIPアドレスではなく、SingleIDのRADIUSサーバ証明書のCNまたはSANと一致するホスト名を設定します。

``` title="FortiGate CLI"
set server "radius-02-0001.poc.singleid.jp"
```

### ca-certの指定について
今回の検証では、FortiGate側で `set ca-cert <string>` を明示的に設定しなくても接続できました。

Fortinetの証明書に関する解説では、FortiOSを含む多くのOSや機器には、信頼されたルートCAのストアがプリインストールされていると説明されています。そのため、FortiGateにプリインストールされた信頼済みCAにLet's EncryptのルートCAまたは中間CAが含まれていたため接続できた可能性があります。

!!! warning
    証明書検証に失敗する場合は、FortiGate側の信頼済みCA、FortiOSバージョン、証明書チェーン、時刻設定を確認してください。環境によっては、Fortinet公式ドキュメントに従って `ca-cert` の明示指定が必要になる可能性があります。

### SingleIDのサーバ証明書について
RadSecでFortiGateがクライアント証明書として利用するSingleIDのサーバ証明書は、追加費用なしで発行できます。

また、検証時点では、SingleIDのサーバ証明書の発行数に上限はありません。

## 参考情報
* Fortinet公式ドキュメント: [Add RADSEC client support](https://docs.fortinet.com/document/fortigate/7.4.0/new-features/729374/add-radsec-client-support){ target=_blank }
* Fortinet Community: [Troubleshooting Tip: A guide to FortiGate and certificate issues](https://community.fortinet.com/fortigate-3/troubleshooting-tip-a-guide-to-fortigate-and-certificate-issues-141232){ target=_blank }
* SingleID設定ガイド: [リモートアクセスVPN-2要素認証（パスワード認証＋クライアント証明書認証）（IKEv2自動トランスポートモード）](vpn-group-password-cert-ikev2-adv.md)
