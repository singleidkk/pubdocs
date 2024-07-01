# クラウドRADIUS（RADIUS-as-a-Service）
## クラウドRADIUSで認証可能なネットワーク機器は何ですか？
RADIUSをサポートしているネットワーク機器であれば、認証可能です。

標準的なRADIUS属性に加えて、ベンダー固有アトリビュート（VSA）については、以下のベンダーの属性を設定することができます。これは、個々の属性が動作することを保証するものではないことにご注意ください。

* aerohive
* airespace
* arista
* aruba
* bluecoat
* brocade
* checkpoint
* chillispot
* cisco
* cisco.asa
* citrix
* clavister
* extreme
* f5
* fortinet
* H3C
* hp
* juniper
* meraki
* meru
* microsoft
* netscreen
* paloalto
* riverbed
* ruckus
* sonicwall
* yubico
* yamaha

2024年3月16日現在

## クラウドRADIUSがサポートする認証方式は何ですか？

* PAP
* CHAP
* MSCHAP
* EAP-TLS
* EAP-TTLS-PAP
* PEAP-MSCHAPv2

ただし、ワンタイムパスワード認証の場合は、以下となります。

* PAP
* EAP-TTLS-PAP

## RADIUSアカウンティングをサポートしていますか？

サポートしていません。

## クラウドRADIUSは、RADIUS/TLS (or RadSec)をサポートしていますか？

はい、サポートしています。拡張RADIUSサーバをご利用いただくことで、RadSecによる接続が可能です。

## RADIUSサーバには、IPv6のIPアドレスからも接続可能でしょうか？

はい、接続可能です。標準RADIUSサーバをご利用いただくことで、IPv6アドレスからの接続が可能です。IPv6インターネット接続回線をご利用の場合でも、IPv4 over IPv6の場合には、拡張RADIUSサーバを利用して接続可能です。

## 利用しているインターネット接続回線では、固定グローバルIPアドレスが割り当てられていません。クラウドRADIUSを利用できますか？ 

はい、利用できます。
拡張RADIUSサーバを利用すれば、ダイナミックDNSサービスも不要です。

## 利用可能なインターネット接続の回線に制限はありますか？

制限はございません。
ただし、RADIUSはUDP通信となりますので、不安定な回線では認証が正しく行われない場合があります。