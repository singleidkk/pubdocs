# よくある質問
## SAML設定を解除するにはどうしたらよいですか。
SAML設定を解除したいカスタムドメインを指定して、以下のコマンドを実行します。

``` powershell title="PowerShell"
Set-MsolDomainAuthentication -DomainName <カスタムドメイン> -Authentication managed
```
