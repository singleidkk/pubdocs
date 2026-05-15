# クライアント接続設定

## Windows

!!! info
    **簡易設定**は、接続テストや一時的な検証向けに、Windowsの接続画面から認証方式を選択して接続する手順です。本番運用、複数端末への展開、EAP方式・サーバ証明書の検証・認証モードなどを明示的に固定したい場合は、**詳細設定**を使用してください。

    簡易設定では、RADIUSサーバのサーバ証明書が更新されたときに、接続先を確認する画面が再度表示される場合があります。SingleIDのRADIUSサーバ証明書はLet's Encryptで発行されるため、短い周期で定期的に更新されます。本番運用では、詳細設定でRADIUSサーバのホスト名と信頼するCAを明示し、サーバ証明書を検証する設定にしてください。

### 無線LAN接続
#### パスワード認証
[パスワードで無線LANアクセスの認証](./windows_setup_wlan_password.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. 接続設定
    - [簡易設定](./windows_setup_wlan_cert.md)
    - [詳細設定](./windows_setup_wlan_cert2.md)

### 有線LAN接続
#### パスワード認証
- 手順1. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順2. [パスワードで有線LANアクセスの認証](./windows_setup_wired_password.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順3. 接続設定
    - [簡易設定](./windows_setup_wired_cert.md)
    - [詳細設定](./windows_setup_wired_cert2.md)

## iOS/iPadOS
### 無線LAN接続
#### クライアント証明書認証

- 手順1. [クライアント証明書のインストール](./ios_ipados_install_cert.md)
- 手順2. [クライアント証明書で無線LANアクセスの認証](./ios_ipados_setup_wlan_cert.md)

## Android
### 無線LAN接続
#### クライアント証明書認証

- 手順1. [クライアント証明書（Wi-Fi証明書）のインストール](./android_install_cert.md)
- 手順2. [クライアント証明書（Wi-Fi証明書）で無線LANアクセスの認証](./android_setup_wlan_cert.md)
