# 一般的な考慮事項

## SAML設定について 
* SingleIDからM365にユーザおよびグループの同期が完了するまで、シングルサインオンを利用できません。
* シングルサインオンが有効になっている場合、シングルサインオンを構成しているカスタムドメイン内のすべてのユーザーが影響を受けます。シングルサインオンを有効にすると、ユーザはSingleIDで認証され、M365のパスワードでの認証は利用できなくなります。
* M365のシングルサインオン設定を正常に完了するには、M365で全体管理者アカウントを使用する必要があります。
* M365で定義されている既定のドメインは、シングルサインオンに使用されるカスタムドメインであってはなりません。通常、xxxxx.onmicrosoft.comドメインを既定のドメインに設定します。
* M365のカスタムドメインがフェデレーションされると、従業員がメールにアクセスするために使用する Microsoftアプリケーション、特に古いレガシアプリケーションの動作が異なる場合があります。