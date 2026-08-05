# Authentication Survivabilityの利用について
文書更新日:2026-08-05

SingleIDでは、Aruba InstantのAuthentication Survivabilityを使用できません。

## 理由

SingleIDは、共通のルートCA配下に顧客ごとの中間CAを用意し、各中間CAからクライアント証明書を発行します。

AP-505-JPのAruba Instant 8.13.3.0 LSRでAuthentication Survivabilityを検証した結果、同じSingleIDルートCA配下の別の中間CAから発行されたクライアント証明書でも、Aruba Instantの内部RADIUSによるEAP-TLS認証に成功しました。検証した構成では、クライアント証明書の発行元を顧客ごとの中間CAに限定できませんでした。

このため、Authentication Survivabilityを使用すると、対象顧客とは異なる中間CAから発行されたクライアント証明書を受け入れる可能性があります。SingleIDが顧客ごとに設けている証明書の信頼範囲を維持できないため、本機能は使用できません。

## 参考情報

* [Authentication Survivability（Aruba Instant）](https://arubanetworking.hpe.com/techdocs/Archived/Instant-AOS-8/Instant_811_WebHelp/Content/instant-ug/authentication/conf-auth-surviv.htm){ target=_blank }
