# クラウドRADIUS（RADIUS-as-a-Service）
<!-- wp:heading -->
<h2> クラウドRADIUSで認証可能なネットワーク機器は何ですか？ </h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>RADIUSをサポートしているネットワーク機器であれば、認証可能です。<br>標準的なRADIUS属性に加えて、ベンダー固有アトリビュート（VSA）については、以下のベンダーの属性を設定することができます。これは、個々の属性が動作することを保証するものではないことにご注意ください。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>aerohive</li><li>airespace</li><li>arista</li><li>aruba</li><li>bluecoat</li><li>brocade</li><li>checkpoint</li><li>chillispot</li><li>cisco</li><li>cisco.asa</li><li>citrix</li><li>clavister</li><li>extreme</li><li>f5</li><li>fortinet</li><li>hp</li><li>juniper</li><li>meraki</li><li>meru</li><li>microsoft</li><li>netscreen</li><li>paloalto</li><li>riverbed</li><li>ruckus</li><li>sonicwall</li><li>yubico</li><li>yamaha</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>2021年11月21日現在</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2> クラウドRADIUSがサポートする認証方式は何ですか？ </h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul><li>PAP</li><li>CHAP</li><li>MSCHAP</li><li>EAP-TLS</li><li>EAP-TTLS-PAP</li><li>PEAP-MSCHAPv2</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>ただし、ワンタイムパスワード認証の場合は、以下となります。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>PAP</li><li>EAP-TTLS-PAP</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2>クラウドRADIUSは、RADIUS/TLS (or RadSec)をサポートしていますか？</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>サポートしていません。</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>利用しているインターネット接続回線では、固定グローバルIPアドレスが割り当てられていません。クラウドRADIUSを利用できますか？ </h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>クラウドRADIUSは、固定グローバルIPアドレスの契約がないインターネット接続環境でも利用できます。固定グローバルIPアドレスがない環境の場合には、他社が提供するダイナミックDNSサービスを活用することで、クラウドRADIUSが利用できます。ダイナミックDNSサービスについては、販売パートナー様へご相談ください。 </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>利用可能なインターネット接続の回線に制限はありますか？</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>インターネット接続回線に接続する機器に、グローバルIPアドレスが割り当てられている場合には、固定グローバルIPアドレス、動的グローバルIPアドレスに関わらず、利用することができます。ただし、動的グローバルIPアドレスが、割り当てられている場合には、他社が提供するダイナミックDNSサービスをご活用ください。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> インターネット接続回線に接続する機器に、 グローバルIPアドレスではなく、プライベートアドレスが割り当てられている場合には、以下の理由から、グローバルIPアドレスが割り当てられるインターネット接続回線をご契約してください。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>接続先に応じて、接続元のグローバルIPアドレスが変更される場合があります。スマートフォンのデザリングを利用した場合などが該当します。（たとえば、米国のサイトへ接続するときには、a.a.a.a、日本のサイトへ接続するときには、b.b.b.b　のグローバルIPになるなど。）</li><li>グローバルIPアドレスを複数の組織で共有することになるため、クラウドRADIUSへ接続する組織を制限できなくなります。</li></ul>
<!-- /wp:list -->