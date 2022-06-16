<!-- wp:heading {"level":1} -->
<h1>目的</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>FortiGateのリモートアクセスVPN（IPsec）について、以下の要件を満たすために、SingleIDを利用します。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>RADIUSサーバに登録されたユーザでログインする。</li><li>特定グループに属するユーザのみアクセスを許可する。</li><li>ログインの認証方式は、パスワード認証とする。</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 id="env">環境</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2>グループの情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>グループ名</td><td>グループの説明</td><td>メンバー</td></tr><tr><td>リモートアクセスユーザ（ビルトイングループ）</td><td>FortiGateのリモートアクセスVPNへログインできるユーザのグループ</td><td>user1</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2>RADIUSの情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>設定に必要な情報</td><td>説明および情報取得の方法など</td></tr><tr><td>RADIUSサーバのホスト名</td><td> SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「ホスト名」です。 </td></tr><tr><td>RADIUSサーバのIPアドレス</td><td>SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「IPアドレス」です。</td></tr><tr><td>RADIUSサーバのポート番号</td><td> SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「RADIUSポート番号」です。デフォルトUDP1812です。 </td></tr><tr><td>RADIUSクライアントのIPアドレス</td><td>FortiGate側のグローバルIPアドレスです。インターネットに出ていくときの送信元のIPアドレスです。</td></tr><tr><td>RADIUSクライアントのシークレット</td><td>任意の文字列を設定します。ここでは、「fortigate」とします。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2>VPN設定の情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>設定に必要な情報</td><td>説明および情報取得の方法など </td></tr><tr><td>着信インターフェース</td><td>FortiGateのWAN側のインターフェースです。環境により異なります。</td></tr><tr><td>VPNゲートウェイIPアドレス</td><td>VPNクライアントが接続するIPアドレスです。環境により異なります。</td></tr><tr><td>ローカルインターフェース</td><td> FortiGateのLAN側のインターフェースです。環境により異なります。 </td></tr><tr><td>VPNクライアントIPアドレス範囲</td><td>VPNクライアントに割り当てるIPアドレスの範囲です。環境により異なります。</td></tr><tr><td>VPN方式</td><td>IPsec VPN</td></tr><tr><td>認証方式</td><td>事前共有鍵</td></tr><tr><td>事前共有鍵</td><td>任意の文字列を設定します。ここでは、「fortigate」とします。 </td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":1} -->
<h1>設定方法</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2>SingleIDの設定</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>SingleIDの管理者ポータルへログインします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.singleid.jp/product-login/">https://www.singleid.jp/product-login/</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>ユーザの作成</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>  SingleID 管理者ポータル＞ユーザ一覧画面へ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1486,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/08/image-7.png" alt="" class="wp-image-1486"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>ユーザ登録画面の「基本情報」を入力します。 作成するユーザは、<a href="#env" data-type="internal" data-id="#env">環境/グループの情報</a>を参照します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 「グループ」タブをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2184,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-1024x476.png" alt="" class="wp-image-2184"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>参加するグループを選択し、「登録」をクリックします。参加するグループについては、<a href="#env" data-type="internal" data-id="#env">環境/グループの情報</a>を参照します。 </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2253,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-18-1024x311.png" alt="" class="wp-image-2253"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3>RADIUSの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> SingleID 管理者ポータル＞認証＞RADIUS画面へ移動します。「FortiGate」タブへ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2190,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-2-1024x228.png" alt="" class="wp-image-2190"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3> </h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>RADIUSクライアントに関する以下の項目を入力します。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td></td></tr><tr><td>IPアドレス</td><td> <a style="font-family: inherit; font-size: var(--vk-size-text-sm); font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのIPアドレス」です。</td></tr><tr><td>シークレット</td><td> <a style="font-family: inherit; font-size: var(--vk-size-text-sm); font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのシークレット」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:vk-blocks/alert {"style":"warning"} -->
<div class="wp-block-vk-blocks-alert alert alert-warning"><p>選択するサーバの番号により、RADIUSサーバのポート番号が異なります。<br>サーバが１の場合には、UDP1812です。<br>SingleID 管理者ポータル＞認証＞RADIUS＞基本情報タブ画面の「RADIUSポート番号」にサーバの番号と通信ポート番号の対応が記載されています。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:paragraph -->
<p>「VPNアクセスの認証」タブへ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2191,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-3-1024x452.png" alt="" class="wp-image-2191"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>許可グループの設定で「リモートアクセスユーザ」グループをダブルクリックし、許可へ移動させます。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2256,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-19-1024x636.png" alt="" class="wp-image-2256"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2> FortiGateの設定</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>FortiGateの管理GUIへログインします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>https://&lt;FortiGateの管理IPアドレス&gt;</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2196,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-5.png" alt="" class="wp-image-2196"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 id="yamahaadminaccess">RADIUSサーバの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> FortiGate管理GUI＞ ユーザ＆認証＞RADIUSサーバへ移動します。「新規作成」ボタンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>以下の項目を設定し、「OK」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>SingleID_RADIUS</td></tr><tr><td>IP/名前</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSサーバのIPアドレス」です。</td></tr><tr><td>シークレット</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのシークレット」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2199,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-7.png" alt="" class="wp-image-2199"/></figure>
<!-- /wp:image -->

<!-- wp:vk-blocks/alert {"style":"warning"} -->
<div class="wp-block-vk-blocks-alert alert alert-warning"><p> 「接続をテスト」をクリックし、接続が成功しない場合には、以下をご確認ください。<br>・設定内容が間違いないこと<br>・FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること <br><br>SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、「ユーザクレデンシャルをテスト」ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。  </p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:heading {"level":3} -->
<h3>ユーザグループの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>FortiGate管理GUI＞ユーザ＆認証＞ユーザグループへ移動します。「新規作成」ボタンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>以下の項目を設定し、「OK」を クリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>SingleID</td></tr><tr><td>タイプ</td><td>ファイアウォール</td></tr><tr><td>リモートグループ</td><td>リモートサーバ：SingleID_RADIUS　グループ：すべて</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2202,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-9.png" alt="" class="wp-image-2202"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3> VPNの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>FortiGate管理GUI＞VPN＞IPsecウィザードへ移動します。「新規作成＞管理者」ボタンをクリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「次へ」を クリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>IPsecVPN</td></tr><tr><td>テンプレートタイプ</td><td>リモートアクセス</td></tr><tr><td>リモートデバイスタイプ</td><td>クライアントベース　FortiClient</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2258,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-20-1024x280.png" alt="" class="wp-image-2258"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「次へ」を クリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>着信インターフェース</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「着信インターフェース」です。</td></tr><tr><td>認証方式</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「認証方式」です。</td></tr><tr><td>事前共有鍵</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「事前共有鍵」です。</td></tr><tr><td>ユーザグループ</td><td>SingleID</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2261,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-21-1024x279.png" alt="" class="wp-image-2261"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「次へ」を クリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>ローカルインターフェース</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「ローカルインターフェース」です。</td></tr><tr><td>ローカルアドレス</td><td>LAN側ネットワークを指定（例：internal）</td></tr><tr><td>クライアントアドレス範囲</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「VPNクライアントIPアドレス範囲」です。</td></tr><tr><td>サブネットマスク</td><td>255.255.255.255</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2267,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-24-1024x321.png" alt="" class="wp-image-2267"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> 「次へ」を クリック します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2265,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-23-1024x290.png" alt="" class="wp-image-2265"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>設定を確認し、「作成」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1>動作確認方法</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2> FortiClientのインストール</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>以下、URLからFortiClientをダウンロードして、インストールします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.forticlient.com" target="_blank" rel="noreferrer noopener">https://www.forticlient.com</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>FortiClientの設定</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>VPN</td><td>IPsec VPN</td></tr><tr><td>接続名</td><td>任意です。例：SingleID</td></tr><tr><td>リモートGW</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「VPNゲートウェイIPアドレス」です。</td></tr><tr><td>認証方法</td><td><a style="font-family: inherit; font-size: revert; font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/VPN設定の情報</a>　の「認証方式」です。</td></tr><tr><td>認証（XAuth）</td><td>ユーザ名入力</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2273,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-26.png" alt="" class="wp-image-2273"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>FortiGateへリモートアクセスVPN</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>SingleIDのユーザ（user1）でログインを試みます。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2288,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-27.png" alt="" class="wp-image-2288"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>user1でGUIへログインできたことを確認します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2289,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-28.png" alt="" class="wp-image-2289"/></figure>
<!-- /wp:image -->