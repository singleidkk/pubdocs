<!-- wp:heading {"level":1} -->
<h1>目的</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>FortiGateの管理GUIへのアクセスについて、以下の要件を満たすために、SingleIDを利用します。</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>RADIUSサーバに登録されたユーザでログインする。</li><li>特定グループに属するユーザのみアクセスを許可する。</li><li>ログインの認証方式は、パスワード認証とする。</li><li>閲覧権限（super_admin_readonly）でログインする。設定変更は不可とする。</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 id="env">環境</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2>グループの情報</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td>グループ名</td><td>グループの説明</td><td>メンバー</td></tr><tr><td>システム管理者（ビルトイングループ）</td><td>FortiGateへ「 super_admin_readonly 」でログインできるユーザのグループ</td><td>user1</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2>RADIUSの情報</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td>設定に必要な情報</td><td>説明および情報取得の方法など</td></tr><tr><td>RADIUSサーバのホスト名</td><td> SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「ホスト名」です。 </td></tr><tr><td>RADIUSサーバのIPアドレス</td><td>SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「IPアドレス」です。</td></tr><tr><td>RADIUSサーバのポート番号</td><td> SingleID 管理者ポータル＞認証＞RADIUS画面の基本情報タブの「RADIUSポート番号」です。デフォルトUDP1812です。 </td></tr><tr><td>RADIUSクライアントのIPアドレス</td><td>FortiGate側のグローバルIPアドレスです。インターネットに出ていくときの送信元のIPアドレスです。</td></tr><tr><td>RADIUSクライアントのシークレット</td><td>任意の文字列を設定します。ここでは、シークレットを「fortigate」とします。</td></tr></tbody></table></figure>
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

<!-- wp:image {"id":2188,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-1-1024x311.png" alt="" class="wp-image-2188"/></figure>
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
<figure class="wp-block-table is-style-regular"><table><tbody><tr><td>項目</td><td></td></tr><tr><td>IPアドレス</td><td> <a style="font-family: inherit; font-size: var(--vk-size-text-sm); font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSサーバのIPアドレス」です。</td></tr><tr><td>シークレット</td><td> <a style="font-family: inherit; font-size: var(--vk-size-text-sm); font-weight: inherit;" href="#env" data-type="internal" data-id="#env">環境/RADIUSの情報</a>　の「RADIUSクライアントのシークレット」です。</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:vk-blocks/alert {"style":"warning"} -->
<div class="wp-block-vk-blocks-alert alert alert-warning"><p>選択するサーバの番号により、RADIUSサーバのポート番号が異なります。<br>サーバが１の場合には、UDP1812です。<br>SingleID 管理者ポータル＞認証＞RADIUS＞基本情報タブ画面の「RADIUSポート番号」にサーバの番号と通信ポート番号の対応が記載されています。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:paragraph -->
<p>「管理者アクセスの認証」タブへ移動します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2191,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-3-1024x452.png" alt="" class="wp-image-2191"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>許可グループの設定で「システム管理者」グループをダブルクリックし、許可へ移動させます。「super_admin_readonly」の権限を選択します。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>「登録」をクリックします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2192,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-4-1024x632.png" alt="" class="wp-image-2192"/></figure>
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

<!-- wp:paragraph -->
<p> FortiGate管理GUI＞システム＞管理者へ移動します。「新規作成＞管理者」ボタンをクリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> 以下の項目を設定し、「OK」を クリックします。 </p>
<!-- /wp:paragraph -->

<!-- wp:table {"className":"is-style-vk-table-border"} -->
<figure class="wp-block-table is-style-vk-table-border"><table><tbody><tr><td>項目</td><td>設定値</td></tr><tr><td>ユーザ名</td><td>SingleID</td></tr><tr><td>タイプ</td><td>リモートサーバグループのすべてのユーザと一致</td></tr><tr><td>管理者プロファイル</td><td>admin_no_access</td></tr><tr><td>リモートユーザグループ</td><td>SingleID</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:image {"id":2203,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-10.png" alt="" class="wp-image-2203"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>CLIから以下のコマンドで、設定します。</p>
<!-- /wp:paragraph -->

<!-- wp:vk-blocks/alert -->
<div class="wp-block-vk-blocks-alert alert alert-info"><p>この設定を行うことで、SingleIDグループに設定した「admin_no_access」プロファイルをRADIUSサーバで指定したプロファイルの「super_admin_readonly」で上書きできます。</p></div>
<!-- /wp:vk-blocks/alert -->

<!-- wp:loos-hcb/code-block {"isLineShow":"on"} -->
<div class="hcb_wrap"><pre class="prism on-numbers lang-plain"><code># config system admin
     edit &quot;SingleID&quot;
     set accprofile-override enable
end</code></pre></div>
<!-- /wp:loos-hcb/code-block -->

<!-- wp:heading {"level":4} -->
<h4>参考</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p> Fortinet社の以下の情報を参考にさせていただきました。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a rel="noreferrer noopener" href="https://kb.fortinet.com/kb/documentLink.do?externalID=FD36127" target="_blank">Technical Tip: Remote admin login with Radius selecting admin access account profile</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1>動作確認方法</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2> FortiGateの管理GUIへログイン</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>https://&lt;FortiGateの管理IPアドレス&gt;　へアクセスします。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2196,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-5.png" alt="" class="wp-image-2196"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>SingleIDのユーザ（user1）でログインを試みます。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>user1でGUIへログインできたことを確認します。</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":2211,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-14.png" alt="" class="wp-image-2211"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>補足</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>以下の画面が表示されたときには、CLIで設定を確認し、「set accprofile-override enable」がセットされていることを確認してください。</p>
<!-- /wp:paragraph -->

<!-- wp:loos-hcb/code-block {"dataLineNum":"13"} -->
<div class="hcb_wrap"><pre class="prism undefined-numbers lang-plain" data-line="13"><code>TEST-FGT-A # config system admin

TEST-FGT-A (admin) # edit &quot;SingleID&quot;

TEST-FGT-A (SingleID) # show
config system admin
    edit &quot;SingleID&quot;
        set remote-auth enable
        set accprofile &quot;admin_no_access&quot;
        set vdom &quot;root&quot;
        set wildcard enable
        set remote-group &quot;SingleID&quot;
        set accprofile-override enable
    next
end</code></pre></div>
<!-- /wp:loos-hcb/code-block -->

<!-- wp:image {"id":2209,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://www.singleid.jp/wp-content/uploads/2021/11/image-12.png" alt="" class="wp-image-2209"/></figure>
<!-- /wp:image -->