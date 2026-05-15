# クライアント接続設定

## Windows

!!! info
    **簡易設定**は、接続テストや一時的な検証向けに、Windowsの接続画面から認証方式を選択して接続する手順です。本番運用、複数端末への展開、EAP方式・サーバ証明書の検証・認証モードなどを明示的に固定したい場合は、**詳細設定**を使用してください。

    簡易設定では、RADIUSサーバのサーバ証明書が更新されたときに、接続先を確認する画面が再度表示される場合があります。SingleIDのRADIUSサーバ証明書はLet's Encryptで発行されるため、短い周期で定期的に更新されます。本番運用では、詳細設定でRADIUSサーバのホスト名と信頼するCAを明示し、サーバ証明書を検証する設定にしてください。

    パスワード認証の簡易設定も、Windowsの接続画面で接続時にサーバ証明書を確認する流れです。本番運用でEAP方式やサーバ証明書の検証設定を固定したい場合は、詳細設定を使用してください。複数端末へ同じ設定を展開する場合は、グループポリシーやMDMによる設定配布を推奨します。

### 無線LAN接続
#### パスワード認証
- 接続設定
    - [簡易設定](./windows_setup_wlan_password.md)
    - [詳細設定](./windows_setup_wlan_password2.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. 接続設定
    - [簡易設定](./windows_setup_wlan_cert.md)
    - [詳細設定](./windows_setup_wlan_cert2.md)

### 有線LAN接続
#### パスワード認証
- 手順1. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順2. 接続設定
    - [簡易設定](./windows_setup_wired_password.md)
    - [詳細設定](./windows_setup_wired_password2.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順3. 接続設定
    - [簡易設定](./windows_setup_wired_cert.md)
    - [詳細設定](./windows_setup_wired_cert2.md)

## iOS/iPadOS

!!! info
    iOS/iPadOSの手動設定は、接続テストや少数端末での利用に使用できます。本番運用や複数端末への展開では、構成プロファイルまたはMDMでSSID、EAP-TLS、RADIUSサーバ名、信頼するCAを配布することを推奨します。

    手動設定では、接続時に表示されるサーバ証明書をユーザが信頼する流れになります。管理者側でRADIUSサーバ名と信頼するCAを固定できないため、誤操作やなりすましアクセスポイントへの接続リスクを下げるには、構成プロファイルまたはMDMで設定を配布することを推奨します。

### 無線LAN接続
#### クライアント証明書認証

- 手順1. [クライアント証明書のインストール](./ios_ipados_install_cert.md)
- 手順2. [クライアント証明書で無線LANアクセスの認証](./ios_ipados_setup_wlan_cert.md)

## Android

!!! info
    Androidでは、接続設定画面でEAP方式、CA証明書、ドメイン、ユーザ証明書を指定します。本手順は、RADIUSサーバのホスト名と信頼するCAを指定してサーバ証明書を検証する設定です。

    複数端末へ同じ設定を展開する場合は、MDMによる設定配布を推奨します。Android 10以前では、端末やメーカー実装により**ドメイン**を空白にしても接続できる場合がありますが、設定画面に**ドメイン**項目がある場合は、RADIUSサーバのホスト名を入力してください。

### 無線LAN接続
#### クライアント証明書認証

- 手順1. [クライアント証明書（Wi-Fi証明書）のインストール](./android_install_cert.md)
- 手順2. [クライアント証明書（Wi-Fi証明書）で無線LANアクセスの認証](./android_setup_wlan_cert.md)
