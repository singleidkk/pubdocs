<!-- wp:heading {"level":1} -->
<h1 id="block-faa217c5-800c-484d-8e63-c1d68b88e1a2">目的</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-52612b0f-ad31-4374-aa7a-a4bdac9dd73b">FortiGateのリモートアクセスVPN（SSLVPN）について、以下の要件を満たすために、SingleIDを利用します。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul id="block-ffa0b631-ec90-4ed3-982d-a952836b5d90"><li>RADIUSサーバに登録されたユーザでログインする。</li><li>特定グループに属するユーザのみアクセスを許可する。</li><li>組織ごとのプライベートCAで発行したクライアント証明書のみ許可する。</li><li>ログインの認証方式は、パスワード＋クライアント証明書認証とする。</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 id="block-a2dc1425-f883-4710-8363-d6ecc24f1da6">環境</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 id="block-c4ee4011-2547-4bbe-9510-699eec55166d">グループの情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>グループ名</td><td>グループの説明</td><td>メンバー</td></tr><tr><td>リモートアクセスユーザ（ビルトイングループ）</td><td>FortiGateのリモートアクセスVPNへログインできるユーザのグループ</td><td>user1</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2>CAの情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td> 設定に必要な情報 </td><td> 説明および情報取得の方法など </td></tr><tr><td>中間CAの発行者</td><td> SingleID 管理者ポータル＞認証＞証明書画面の基本情報タブの中間CAの「 発行者 」です。 </td></tr><tr><td>中間CAのサブジェクト</td><td> SingleID 管理者ポータル＞認証＞証明書画面の基本情報タブの中間CAの「 サブジェクト 」です。  </td></tr><tr><td>中間CAの失効リスト配布ポイント</td><td> SingleID 管理者ポータル＞認証＞証明書画面の基本情報タブの中間CAの「 失効リストの配布ポイント 」です。  </td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2 id="block-5d278fa1-fdd3-481b-b43f-c128259a1d11">RADIUSの情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>設定に必要な情報</td><td>説明および情報取得の方法など</td></tr><tr><td>RADIUSサーバのホスト名</td><td>SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「ホスト名」です。</td></tr><tr><td>RADIUSサーバのIPアドレス</td><td>SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「IPアドレス」です。</td></tr><tr><td>RADIUSサーバのポート番号</td><td>SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「RADIUSポート番号」です。デフォルトUDP1812です。</td></tr><tr><td>RADIUSクライアントのIPアドレス</td><td>FortiGate側のグローバルIPアドレスです。インターネットに出ていくときの送信元のIPアドレスです。</td></tr><tr><td>RADIUSクライアントのシークレット</td><td>任意の文字列を設定します。ここでは、「fortigate」とします。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2 id="block-4bcc4855-14e0-4be2-9e7f-72cea524d912">VPN設定の情報</h2>
<!-- /wp:heading -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>設定に必要な情報</td><td>説明および情報取得の方法など</td></tr><tr><td> リッスンするインターフェース</td><td>FortiGateのWAN側のインターフェースです。環境により異なります。</td></tr><tr><td> リッスンするポート </td><td>VPNクライアントが接続するポート番号です。ここでは、「10443」とします。</td></tr><tr><td>サーバ証明書</td><td>VPNのサーバ認証のためのサーバ証明書を指定します。ここでは、「Fortinet_Factory」とします。</td></tr><tr><td>ファイアウォールポリシーの宛先</td><td>VPNでアクセスした通信に対して、許可したい宛先です。環境により異なります。</td></tr><tr><td> VPNゲートウェイIPアドレス </td><td> VPNクライアントが接続するIPアドレスです。環境により異なります。 </td></tr><tr><td> 発信インターフェース </td><td>VPN接続が完了し、内部通信が行われるときに、発信元のインターフェースです。 環境により異なります。  </td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":1} -->
<h1 id="block-75a94984-aa3a-4150-a1f8-46f39a0ac8a3">設定方法</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 id="block-94d041ac-2f7e-4620-ad17-d9d0d546aca5">SingleIDの設定</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-631fcbb0-4750-4af8-9c2c-89618e0b1990">SingleIDの管理者ポータルへログインします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-631fcbb0-4750-4af8-9c2c-89618e0b1990">https://login.singleid.jp/</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 id="block-c26851b0-0350-4159-8381-dafe04f0baff">ユーザの作成</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-ab1d329d-6e9f-469c-a650-6b22b984652a">SingleID 管理者ポータル＞ユーザ一覧画面へ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-4ee9048f-5147-4a01-9039-c882ae971323">「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-7ae010e7-a239-4f16-9e0a-37c53789714f"><img src="https://www.singleid.jp/wp-content/uploads/2021/08/image-7.png" alt="画像に alt 属性が指定されていません。ファイル名: image-7.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-c56c28bb-2a61-4740-af3e-c54658388c79">ユーザ登録画面の「基本情報」を入力します。 作成するユーザは、<a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/グループの情報</a>を参照します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-ad557f1a-6ede-4650-ad1b-418272874aac">「グループ」タブをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-c5b57d84-932f-4e1e-8724-7a0057c88f46"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-1024x476.png" alt="画像に alt 属性が指定されていません。ファイル名: image-1024x476.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-21d3441e-9baf-4294-a270-5d2398c652af">参加するグループを選択し、「登録」をクリックします。参加するグループについては、<a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/グループの情報</a>を参照します。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-f68ac9cb-a433-4dc2-8014-7158b9e3ca79"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-18-1024x311.png" alt="画像に alt 属性が指定されていません。ファイル名: image-18-1024x311.png"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3>クライアント証明書の発行</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>SingleID 管理者ポータル＞ユーザ一覧画面へ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>証明書を発行したいユーザ（user1）の行の左端のチェックボックスにチェックし、「選択実行」をクリックします。プルダウンメニューが表示されますので、「証明書の発行」を選択します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2363,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-31-1024x228.png" alt="" class="wp-image-2363"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>証明書のプロファイルを選択します。ここでは、デフォルトのプロファイルを選択し、「発行」をクリックします。クライアント証明書は、ユーザに設定されているメールアドレス宛に送信されます。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1579,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-1.png" alt="" class="wp-image-1579"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3>SingleIDの中間CA証明書のダウンロード</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> クライアント証明書を発行したSingleIDの中間CA証明書をダウンロードします。  </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>SingleID  管理者ポータル ＞認証＞証明書画面へ移動します。「基本情報」タブへ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>中間CAの横の雲のアイコンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:vk-blocks/alert {"style":"danger"} -->
<div class="wp-block-vk-blocks-alert alert alert-danger"><p>誤って、ルートCA証明書をダウンロードしないようにご注意ください。ルートCA証明書のダウンロードは必要ありません。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:image {"id":1599,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-10.png" alt="" class="wp-image-1599"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>「ダウンロード」をクリックし、SingleIDの中間CA証明書をダウンロードします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1601,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-12.png" alt="" class="wp-image-1601"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3>SingleIDの中間CAの証明書失効リスト（CRL）のダウンロード</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> クライアント証明書を発行したSingleIDの中間CAの証明書失効リスト（CRL）をダウンロードします。  </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>SingleID  管理者ポータル ＞認証＞証明書画面へ移動します。「基本情報」タブへ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>中間CAの失効リスト配布ポイントの横の雲のアイコンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:vk-blocks/alert {"style":"danger"} -->
<div class="wp-block-vk-blocks-alert alert alert-danger"><p>誤って、ルートCAの証明書失効リスト（CRL）をダウンロードしないようにご注意ください。ルートCAの証明書失効リスト（CRL）ダウンロードは必要ありません。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:image {"id":2374,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-33.png" alt="" class="wp-image-2374"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>「ダウンロード」をクリックし、SingleIDの中間CAの証明書失効リストをダウンロードします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1601,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-12.png" alt="" class="wp-image-1601"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 id="block-63e87bd2-836d-419d-967e-53e8f08b1a13">RADIUSの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-122429d9-fcea-4305-9a66-65d15a64acbf">SingleID 管理者ポータル＞認証＞RADIUS画面へ移動します。「FortiGate」タブへ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-1aaa811a-749b-4b54-ab09-f95a4465fca1">「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-697b56e1-76b4-456d-a8e2-96dbf49958cb"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-2-1024x228.png" alt="画像に alt 属性が指定されていません。ファイル名: image-2-1024x228.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-09497afd-0625-45ec-8fb8-3d634ee90bf4">RADIUSクライアントに関する以下の項目を入力します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「VPNアクセスの認証」タブへ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>﻿</td></tr><tr><td>IPアドレス</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのIPアドレス」です。</td></tr><tr><td>シークレット</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのシークレット」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:vk-blocks/alert {"style":"warning"} -->
<div class="wp-block-vk-blocks-alert alert alert-warning"><p>選択するサーバの番号により、RADIUSサーバのポート番号が異なります。<br>サーバが１の場合には、UDP1812です。<br>SingleID 管理者ポータル＞認証＞RADIUS＞基本情報タブ画面の「RADIUSポート番号」にサーバの番号と通信ポート番号の対応が記載されています。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:paragraph -->
<p id="block-bdfc678e-6ead-429b-8e26-63b01e0ea0da"></p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-30f47898-699b-4ac0-9b39-b5187d6afee3"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-3-1024x452.png" alt="画像に alt 属性が指定されていません。ファイル名: image-3-1024x452.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-d9d1e58e-95ab-46e0-b80e-4be578f24d1c">許可グループの設定で「リモートアクセスユーザ」グループをダブルクリックし、許可へ移動させます。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-9bd40d64-a9aa-4304-ad9c-739ddc722b46">「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-bfd73f6d-c644-43f2-b8ec-da06bc2efc10"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-19-1024x636.png" alt="画像に alt 属性が指定されていません。ファイル名: image-19-1024x636.png"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 id="block-1a4b9cd1-3307-4b55-8415-738edc3c521a">FortiGateの設定</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-aeb819dd-0233-4083-a381-25855e7c8c53">FortiGateの管理GUIへログインします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://<FortiGateの管理IPアドレス>">https://&lt;FortiGateの管理IPアドレス&gt;</a></p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-b57d17a1-0d05-4ea2-8357-696cbbfef102"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-5.png" alt="画像に alt 属性が指定されていません。ファイル名: image-5.png"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3>中間CAの証明書のインポート</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> FortiGate管理GUI＞システム＞証明書へ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 「Create/Import」＞CA証明書をクリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>   以下の項目を設定し、「OK」を クリックします </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>タイプ</td><td>ファイル</td></tr><tr><td>アップロード</td><td> SingleID管理者ポータルからダウンロードした中間CAの証明書</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2379,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-34.png" alt="" class="wp-image-2379"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3> 中間CA の証明書失効リスト（CRL）のインポート</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> FortiGate管理GUI＞システム＞証明書へ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「Create/Import」＞CRLをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「OK」を クリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>インポート方式</td><td>オンライン更新</td></tr><tr><td>HTTPサーバのURL</td><td> <a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/CAの情報</a>　の「中間CAの失効リスト配布ポイント」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2369,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-32.png" alt="" class="wp-image-2369"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>定期的（１時間ごと）に証明書失効リスト（CRL）を更新するために、CLIから「set update-interval 3600」を以下のように設定します。</p>
<!-- /wp:paragraph -->

<!-- wp:loos-hcb/code-block -->
<div class="hcb_wrap"><pre class="prism undefined-numbers lang-plain"><code>config vpn certificate crl
    edit &quot;CRL_1&quot;
        set update-interval 3600
    next
end</code></pre></div>
<!-- /wp:loos-hcb/code-block -->

<!-- wp:heading {"level":3} -->
<h3 id="block-15242965-7690-424b-8c6c-1d71c1e85dd3">RADIUSサーバの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-b5da3f3a-a7cb-428d-95b7-2b5a578ed798">FortiGate管理GUI＞ ユーザ＆認証＞RADIUSサーバへ移動します。「新規作成」ボタンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-ae449e06-4e67-4939-84f8-18b8a1fc9190">以下の項目を設定し、「OK」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>SingleID_RADIUS</td></tr><tr><td>IP/名前</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/RADIUSの情報</a>　の「RADIUSサーバのIPアドレス」です。</td></tr><tr><td>シークレット</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのシークレット」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-35672eee-34c5-47c1-bb35-8ba1c7d8d808"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-7.png" alt="画像に alt 属性が指定されていません。ファイル名: image-7.png"/></figure>
<!-- /wp:image -->

<!-- wp:vk-blocks/alert {"style":"warning"} -->
<div class="wp-block-vk-blocks-alert alert alert-warning"><p>「接続をテスト」をクリックし、接続が成功しない場合には、以下をご確認ください。<br>・設定内容が間違いないこと<br>・FortiGateからSingleIDのRADIUSサーバのIPアドレスへ接続可能であること<br><br>SingleIDのRADIUSサーバでは、FortiGateのVPNアクセスや管理アクセスのみ認証可能となるように制限をかけているため、「ユーザクレデンシャルをテスト」ボタンをクリックし、正しいユーザ情報を入力しても成功となりませんが、問題ではありません。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:heading {"level":3} -->
<h3>PKIユーザの登録</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>以下のようにCLIからPKIユーザを登録します。これにより、VPNクライアントから提供されたクライアント証明書が、登録した中間CAで発行されたクライアント証明書であるかどうかを確認します。</p>
<!-- /wp:paragraph -->

<!-- wp:loos-hcb/code-block -->
<div class="hcb_wrap"><pre class="prism undefined-numbers lang-plain"><code>config user peer
    edit &quot;SingleID_Cert_Check&quot;
        set ca &quot;CA_Cert_1&quot;
    next
end</code></pre></div>
<!-- /wp:loos-hcb/code-block -->

<!-- wp:heading {"level":3} -->
<h3 id="block-02217d43-4f78-4629-ae8a-29752c4e0464">ユーザグループの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-511b9d68-0f99-4016-a026-c0e618c290b7">FortiGate管理GUI＞ユーザ＆認証＞ユーザグループへ移動します。「新規作成」ボタンをクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-dc63134c-7aa1-497f-8bf1-2431c1de6b81">以下の項目を設定し、「OK」を クリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>SingleID_Cert</td></tr><tr><td>タイプ</td><td>ファイアウォール</td></tr><tr><td>リモートグループ</td><td>リモートサーバ：SingleID_RADIUS　グループ：すべて</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image -->
<figure class="wp-block-image" id="block-4a30e056-f021-466b-aa72-f10613038065"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-9.png" alt="画像に alt 属性が指定されていません。ファイル名: image-9.png"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 id="block-0f0174cf-5518-4250-bf3b-e60c611b2bd1">SSLVPNの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-45daeb5f-206a-401e-8cf7-5c557757eb03">FortiGate管理GUI＞VPN＞SSL-VPN設定へ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p id="block-0c2d0d49-01fe-457e-87dd-945645ccc272">以下の項目を設定し、「適用」を クリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>リッスンするインターフェース</td><td> <a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「リッスンするインターフェース 」です。</td></tr><tr><td>リッスンするポート</td><td> <a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「リッスンするポート 」です。</td></tr><tr><td>サーバ証明書</td><td> <a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「サーバ証明書 」です。</td></tr><tr><td>クライアント証明書を要求</td><td>有効</td></tr><tr><td>認証ポータルマッピング</td><td>グループ：SingleID_Cert　ポータル：full-access<br>すべてのその他のユーザ/グループ　ポータル：web-access</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2385,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/2021-11-08_5-01-45-3.png" alt="" class="wp-image-2385"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>CLIから以下を設定します。</p>
<!-- /wp:paragraph -->

<!-- wp:loos-hcb/code-block -->
<div class="hcb_wrap"><pre class="prism undefined-numbers lang-plain"><code>config vpn ssl settings
    config authentication-rule
        edit 1
            set client-cert enable
            set user-peer &quot;SingleID_Cert_Check&quot;
        next
    end
end</code></pre></div>
<!-- /wp:loos-hcb/code-block -->

<!-- wp:heading {"level":3} -->
<h3>ポリシーの設定</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>SSLVPNの設定を適用すると、上部に以下のようなメッセージが表示されます。リンクをクリックし、ファイアウォールポリシーの設定を行います。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>表示されない場合には、 FortiGate管理GUI＞ポリシー＆オブジェクト＞ファイアウォールポリシーへ移動します。 </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2317,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-29.png" alt="" class="wp-image-2317"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-0c2d0d49-01fe-457e-87dd-945645ccc272">以下の項目を設定し、「適用」を クリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>名前</td><td>SSLVPN </td></tr><tr><td>着信インターフェース</td><td>SSL-VPN tunnel Interface (ssl.root)</td></tr><tr><td>発信インターフェース</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「発信インターフェース」です。</td></tr><tr><td>発信元</td><td>アドレス：SSLVPN_TUNNEL_ADDR1　ユーザグループ：SingleID_Cert</td></tr><tr><td>宛先</td><td> <a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「ファイアウォールポリシーの宛先」です。</td></tr><tr><td>サービス</td><td>all</td></tr><tr><td>アクション</td><td>許可</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2318,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/2021-11-08_5-25-13-741x1024.png" alt="" class="wp-image-2318"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":1} -->
<h1 id="block-30aa7cd3-efcb-4d9f-8556-b428e56a6553">動作確認方法</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2>クライアント証明書のダウンロード</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>SingleIDシステム管理から届いたメールを開きます。「ダウンロード」をクリックします。P12形式のクライアント証明書がダウンロードされます。</p>
<!-- /wp:paragraph -->

<!-- wp:vk-blocks/alert -->
<div class="wp-block-vk-blocks-alert alert alert-info"><p>もし、PCでメールを受信し、スマートフォンやタブレットにクライアント証明書をインストールしたい場合には、メールに添付されているQRコードを スマートフォンやタブレットで読み取ります。クライアント証明書のダウンロードURLを認識しますので、そのURLへアクセスすることで、 スマートフォンやタブレット にもクライアント証明書をダウンロードできます。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:image {"id":1878,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/2021-09-02_19-10-04-1024x489-1.png" alt="" class="wp-image-1878"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>クライアント証明書のインストール</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>ダウンロードしたSingleIDのユーザ(user1)の P12形式のクライアント証明書ファイルをダブルクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>保存場所として「現在のユーザー」を選択し、「次へ」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1584,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-3.png" alt="" class="wp-image-1584"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>ダウンロードしたP12形式のファイル名とパスが正しく入力されていることを確認し、「次へ」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1585,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-4.png" alt="" class="wp-image-1585"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>「パスワード」に、クライアント証明書のパスワードを入力します。デフォルトでは、クライアント証明書のパスワードは、ユーザ名と同一です。SingleIDのユーザ(user1)のクライアント証明書のパスワードとして、user1を入力し、「次へ」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1793,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-28.png" alt="" class="wp-image-1793"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>「証明書の種類に基づいて、自動に証明書ストアを選択する」が選択されていることを確認し、「次へ」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1587,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-6.png" alt="" class="wp-image-1587"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>「完了」をクリックし、証明書をインストールします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1589,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-7.png" alt="" class="wp-image-1589"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>クライアント証明書のインストールが成功しました。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":1596,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/09/image-9.png" alt="" class="wp-image-1596"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 id="block-005fcf60-e3c8-4cd3-a91a-831250e392dc">FortiClientのインストール</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-ad3242fe-0035-4bee-b434-d63f82f729e3">以下、URLからFortiClientをダウンロードして、インストールします。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.forticlient.com">https://www.forticlient.com</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 id="block-2b01d3f3-6c87-4963-809f-c5ec077dc4fb">FortiClientの設定</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「保存」を クリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-regular"} -->
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>VPN</td><td>SSL-VPN</td></tr><tr><td>接続名</td><td>任意です。例：SingleID_Cert</td></tr><tr><td>リモートGW</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「VPNゲートウェイIPアドレス」です。</td></tr><tr><td>ポートの編集</td><td><a href="https://www.singleid.jp/wp-admin/post.php?post=2246&amp;action=edit#env">環境/VPN設定の情報</a>　の「リッスンするポート」です。</td></tr><tr><td>クライアント証明書</td><td>インストールしたクライアント証明書を選択します。</td></tr><tr><td>認証</td><td>ユーザ名入力</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2405,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/2021-11-08_9-45-23-1.png" alt="" class="wp-image-2405"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 id="block-f69a353b-3655-4f85-85b2-f65ace3b2e91">FortiGateへリモートアクセスVPN</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="block-242a7512-877b-4d48-99ae-9e4178abe3e7">SingleIDのユーザ（user1）でログインを試みます。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2407,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/2021-11-08_9-50-38.png" alt="" class="wp-image-2407"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p id="block-d66cdb1d-21f8-47d0-b342-1c450a863c9a">user1でログインできたことを確認します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2408,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-37.png" alt="" class="wp-image-2408"/></figure>
<!-- /wp:image -->