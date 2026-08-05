# Aruba Instant Authentication Survivability EAP-TLS評価記録

検証日:2026-08-05

この文書は、Aruba InstantのAuthentication SurvivabilityとSingleIDクライアント証明書を組み合わせた場合の検証手順と結果を記録したものです。公開設定ガイドとしては使用しません。

## 結論

SingleIDでは、Aruba InstantのAuthentication Survivabilityを使用しません。

SingleIDは、共通のルートCA配下に顧客ごとの中間CAを用意しています。検証では、同じSingleIDルートCA配下の別の中間CAから発行されたクライアント証明書でも、Aruba Instantの内部RADIUSによるEAP-TLS認証に成功しました。検証した構成では、クライアント証明書の発行元を顧客ごとの中間CAに限定できませんでした。

## 検証環境

| **項目** | **内容** |
| :--- | :--- |
| 検証機種 | Aruba AP-505-JP（R2H27A） |
| ソフトウェア | Aruba Instant 8.13.3.0 LSR（build 96306） |
| 通常時の認証サーバ | SingleID RADIUSサーバ |
| 障害時の認証サーバ | Aruba Instant内部RADIUS |
| 認証方式 | EAP-TLS |
| クライアント | Windows |

AP-615-JPは検証していません。SingleIDはRADIUS Status-Serverをサポートしていないため、RADIUS障害検知後の自動切り替えは評価対象外です。認証キャッシュによるEAP-TLS認証は、`priority-use-local-cache-auth`を一時的に設定して確認しました。

## 検証構成

Aruba Instantには、以下の証明書を登録しました。

| **用途** | **登録内容** |
| :--- | :--- |
| RADIUS serverのServer証明書 | SingleIDで発行した認証サーバ証明書、秘密鍵、中間CA証明書、ルートCA証明書を含むPEM |
| RADIUS serverのTrusted CA | SingleIDの顧客別中間CA証明書と共通ルートCA証明書を含むPEM |

SingleIDでダウンロードしたP12ファイルの変換には、以下の検証用スクリプトを使用しました。

* Windows: `evaluation/aruba_instant/convert-p12-to-aruba-pem.ps1.txt`
* Linux: `evaluation/aruba_instant/convert-p12-to-aruba-pem.sh.txt`

## 設定手順

### 証明書の登録

1. SingleIDで、RADIUSサーバのFQDNをCNとするサーバ証明書を発行します。
2. P12ファイルを、認証サーバ用PEMとTrusted CA用PEMへ変換します。
3. Aruba Instantの**Maintenance＞Certificates**画面で、認証サーバ用PEMを**Server**として登録します。
4. 顧客別中間CA証明書と共通ルートCA証明書を連結したPEMを、**Trusted CA**として登録します。
5. **Certificate Usage**で、Server証明書とTrusted CAを**RADIUS server**へ割り当てます。

登録時の画面は、以下のファイルに保存しています。

* `docs/images/aruba-instant-upload-server-certificate.png`
* `docs/images/aruba-instant-upload-trusted-ca.png`
* `docs/images/aruba-instant-certificate-assignment.png`
* `docs/images/aruba-instant-certificate-assignment-trusted-ca.png`

### Authentication Survivabilityの設定

以下を設定します。

```text
configure terminal
wlan ssid-profile <SSIDプロファイル名>
 auth-survivability
 exit
auth-survivability cache-time-out 24
end
commit apply
```

認証キャッシュを優先して内部RADIUSによる認証を確認するため、検証中のみ以下を設定します。

```text
configure terminal
wlan ssid-profile <SSIDプロファイル名>
 priority-use-local-cache-auth
 exit
end
commit apply
```

## 検証手順

1. SingleIDのRADIUSサーバへ接続できる状態で、SingleIDクライアント証明書によるEAP-TLS認証を行います。
2. `show auth-survivability cached-info`を実行し、ユーザー情報がキャッシュされたことを確認します。
3. `priority-use-local-cache-auth`を設定します。
4. キャッシュ作成時に使用したクライアント証明書で再接続します。
5. `show ap debug radius-statistics`を実行し、`InternalServer`の`Access-Accept`が増加したことを確認します。
6. `show ap debug auth-trace-buf 100`を実行し、`rad-accept`と`eap-success`が記録されたことを確認します。
7. 同じSingleIDルートCA配下にある別の中間CAから発行されたクライアント証明書で接続します。
8. 接続結果と内部RADIUSの統計を確認します。
9. 検証後、`priority-use-local-cache-auth`を解除します。

```text
configure terminal
wlan ssid-profile <SSIDプロファイル名>
 no priority-use-local-cache-auth
 exit
end
commit apply
```

## 検証結果

| **確認項目** | **結果** |
| :--- | :--- |
| SingleIDのRADIUSサーバによるEAP-TLS認証 | 成功 |
| 認証成功後のユーザー情報のキャッシュ | 成功 |
| キャッシュ作成時の証明書による内部RADIUS認証 | 成功 |
| 別の中間CAから発行された証明書による内部RADIUS認証 | 成功 |
| 中間CA証明書だけをTrusted CAへ割り当てた場合の内部RADIUS認証 | 失敗 |
| 顧客別中間CA証明書と共通ルートCA証明書を連結した場合の内部RADIUS認証 | 成功 |

認証キャッシュは、以下のコマンドで確認しました。

```text
show auth-survivability cached-info
show auth-survivability debug-log 100
```

内部RADIUSによる認証は、以下のコマンドで確認しました。

```text
show ap debug radius-statistics
show ap debug auth-trace-buf 100
```

`show ap debug radius-statistics`では、内部RADIUSによる認証成功時に`InternalServer`の`Access-Accept`が増加しました。`show ap debug auth-trace-buf 100`には、`rad-accept`と`eap-success`が記録されました。

## 判断理由

SingleIDでは、顧客ごとの中間CAによってクライアント証明書の発行範囲を分離しています。Authentication Survivabilityで別の中間CAから発行されたクライアント証明書も認証された場合、この分離を維持できません。

Aruba Instantの公開CLIでは、Authentication Survivabilityに対してクライアント証明書のIssuer、Subject、SANを条件とする拒否ルールを設定する方法を確認できませんでした。このため、現在の検証結果に基づき、本機能はSingleID環境では使用不可と判断します。

## 参考情報

* [Authentication Survivability（Aruba Instant）](https://arubanetworking.hpe.com/techdocs/Archived/Instant-AOS-8/Instant_811_WebHelp/Content/instant-ug/authentication/conf-auth-surviv.htm)
* [Aruba Instant CLI: wlan ssid-profile](https://arubanetworking.hpe.com/techdocs/CLI-Bank/Content/instant/wlan-ssid-prof.htm)
* [Aruba Instant CLI: show auth-survivability](https://arubanetworking.hpe.com/techdocs/CLI-Bank/Content/instant/show-auth-surviv.htm)
