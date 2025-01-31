---
title: サードパーティアプリケーションと接続する
intro: '{% data variables.product.product_name %}のアイデンティティを、OAuth を使うサードパーティのアプリケーションに接続できます。 これらのアプリケーションを認可する際には、そのアプリケーションを信頼するか、誰が開発したのか、そのアプリケーションがどういった種類の情報にアクセスしたいのかを確認すべきです。'
redirect_from:
  - /articles/connecting-with-third-party-applications
  - /github/authenticating-to-github/connecting-with-third-party-applications
  - /github/authenticating-to-github/keeping-your-account-and-data-secure/connecting-with-third-party-applications
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Identity
  - Access management
shortTitle: Third-party applications
---

サードパーティアプリケーションが {% data variables.product.product_name %} ログインであなたを識別したい場合、そのアプリケーションの開発者の連絡先情報と、リクエストされている情報のリストのページが表示されます。

## アプリケーション開発者に連絡する

アプリケーションは、{% data variables.product.product_name %} 以外のサードパーティにより開発されているため、アプリケーションがアクセスを要求しているデータをどう使うかについて、私たちは正確に把握していません。 アプリケーションについて、質問や懸念がある場合は、ページ上部の開発者情報を使って、アプリケーション管理者に連絡できます。

![{% data variables.product.prodname_oauth_app %}オーナー情報](/assets/images/help/platform/oauth_owner_bar.png)

開発者が情報を入力している場合は、ページの右側に、アプリケーションの詳細情報や関連ウェブサイトが表示されます。

![OAuth アプリケーションの情報とウェブサイト](/assets/images/help/platform/oauth_app_info.png)

## アプリケーションのアクセスとデータのタイプ

アプリケーションは、{% data variables.product.product_name %}のデータに対して*読み取り*または*書き込み*アクセスを持つことができます。

- **読み取りアクセス**は、アプリケーションに対してデータを*見る*ことのみを許可します。
- **書き込みアクセス**は、アプリケーションに対してデータを*変更*することを許可します。

### OAuth のスコープについて

*スコープ*は、アプリケーションがパブリックおよび非パブリックのデータへのアクセスをリクエストできる権限について名前を付けたグループです。

{% data variables.product.product_name %} と統合するサードパーティアプリケーションを使用したい場合、そのアプリケーションは、データに対してどういった種類のアクセスが必要になるのかをあなたに通知します。 アプリケーションにアクセスを許可すれば、アプリケーションはあなたの代わりにデータの読み取りや変更といったアクションを行えるようになります。 たとえば `user:email` スコープをリクエストするアプリケーションを使用したい場合、そのアプリケーションはあなたのプライベートのメールアドレスに対してリードオンリーのアクセスを持つことになります。 For more information, see "[About scopes for {% data variables.product.prodname_oauth_apps %}](/apps/building-integrations/setting-up-and-registering-oauth-apps/about-scopes-for-oauth-apps)."

{% tip %}

**メモ:** 現時点では、ソースコードへのアクセスのスコープをリードオンリーにすることはできません。

{% endtip %}

### リクエストされるデータの種類

アプリケーションがリクエストできるデータの種類はいくつかあります。

![OAuth アクセスの詳細](/assets/images/help/platform/oauth_access_types.png)

{% tip %}

**ヒント:** {% data reusables.user-settings.review_oauth_tokens_tip %}

{% endtip %}

| データの種類                | 説明                                                                                                                                                                                                                                                                                                   |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| コミットのステータス            | サードパーティアプリケーションに、コミットのステータスを報告するためのアクセスを許可できます。 コミットステータスのアクセスがあれば、アプリケーションはビルドが特定のコミットに対して成功したかどうかを判定できます。 アプリケーションは、コードにはアクセスできませんが、特定のコミットに対するステータス情報を読み書き<em>できます</em>。                                                                                                              |
| デプロイメント               | デプロイメントステータスにアクセスできれば、アプリケーションは、リポジトリの特定のコミットに対してデプロイメントが成功したかどうかを判断できます。 アプリケーションはコードにアクセスできなくなります。                                                                                                                                                                                                 |
| Gist                  | [Gist](https://gist.github.com) アクセスがあれば、アプリケーションは {% ifversion not ghae %}あなたのパブリックおよび{% else %}内部{% endif %}シークレット Gist の両方を読み書きできます。                                                                                                                                                              |
| フック                   | [webhook](/webhooks) アクセスがあれば、アプリケーションはあなたが管理するリポジトリ上のフックの設定を読み書きできます。                                                                                                                                                                                                                               |
| 通知                    | 通知アクセスがあれば、アプリケーションは Issue やプルリクエストへのコメントなど、あなたの {% data variables.product.product_name %} 通知を読むことができます。 ただし、アプリケーションはリポジトリ内へはアクセスできません。                                                                                                                                                             |
| Organization および Team | Organization および Team のアクセスがあれば、アプリケーションは Organization および Team のメンバー構成へのアクセスと管理ができます。                                                                                                                                                                                                               |
| 個人ユーザデータ              | ユーザデータには、名前、メールアドレス、所在地など、ユーザプロファイル内の情報が含まれます。                                                                                                                                                                                                                                                       |
| リポジトリ                 | リポジトリ情報には、コントリビュータの名前、あなたが作成したブランチ、リポジトリ内の実際のファイルなどが含まれます。 An application can request access to all of your repositories of any visibility level. For more information, see "[About repositories](/repositories/creating-and-managing-repositories/about-repositories#about-repository-visibility)." |
| リポジトリの削除              | アプリケーションはあなたが管理するリポジトリの削除をリクエストできますが、コードにアクセスすることはできません。                                                                                                                                                                                                                                             |

## 更新された権限のリクエスト

アプリケーションは新しいアクセス権限をリクエストできます。 権限の更新を要求する場合、アプリケーションはその違いについて通知します。

![サードパーティアプリケーションのアクセスを変更する](/assets/images/help/platform/oauth_existing_access_pane.png)
