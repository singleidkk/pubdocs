# クライアント接続設定

!!! info
    802.1X認証では、クライアントがRADIUSサーバ証明書を正しく検証することが重要です。本番運用では、信頼するCAとRADIUSサーバのホスト名を指定し、接続先のサーバ証明書を検証する設定を推奨します。

    SingleIDのRADIUSサーバ証明書はLet's Encryptで発行されるため、短い周期で定期的に更新されます。証明書そのものを都度信頼するのではなく、CAとホスト名で検証する構成にしてください。

## Windows

### 無線LAN接続
#### パスワード認証
- 接続設定
    - [CAとホスト名を指定しない場合（接続時に確認）](./windows_setup_wlan_password.md)
    - [CAとホスト名を指定して検証する場合](./windows_setup_wlan_password2.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. 接続設定
    - [CAとホスト名を指定しない場合（接続時に確認）](./windows_setup_wlan_cert.md)
    - [CAとホスト名を指定して検証する場合](./windows_setup_wlan_cert2.md)

### 有線LAN接続
#### パスワード認証
- 手順1. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順2. 接続設定
    - [CAとホスト名を指定しない場合（接続時に確認）](./windows_setup_wired_password.md)
    - [CAとホスト名を指定して検証する場合](./windows_setup_wired_password2.md)

#### クライアント証明書認証
- 手順1. [クライアント証明書のインストール](./windows_install_cert.md)
- 手順2. [Wired AutoConfigサービスの有効化](./windows_setup_wired_auto_config.md)
- 手順3. 接続設定
    - [CAとホスト名を指定しない場合（接続時に確認）](./windows_setup_wired_cert.md)
    - [CAとホスト名を指定して検証する場合](./windows_setup_wired_cert2.md)

## iOS/iPadOS

### 無線LAN接続
#### クライアント証明書認証

- CAとホスト名を指定しない場合（接続時に確認）
    - 手順1. [クライアント証明書のインストール](./ios_ipados_install_cert.md)
    - 手順2. [接続設定](./ios_ipados_setup_wlan_cert.md)
- CAとホスト名を指定して検証する場合
    - [Apple Configurator設定](./ios_ipados_setup_wlan_cert2.md)

## Android

### 無線LAN接続
#### クライアント証明書認証

- 手順1. [クライアント証明書（Wi-Fi証明書）のインストール](./android_install_cert.md)
- 手順2. [CAとホスト名を指定して検証する場合](./android_setup_wlan_cert.md)
