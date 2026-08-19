---
title: "GitHub Actions Node 24 対応状況"
emoji: "👷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [githubactions, nodejs, security]
published: false
---

# はじめに
GitHub Actions で Node 20 を利用していると次のような警告が表示されています。

> Node.js 20 is deprecated. The following actions target Node.js 20 but are being forced to run on Node.js 24: actions/setup-java@v4. For more information see: https://github.blog/changelog/2025-09-19-deprecation-of-node-20-on-github-actions-runners/

例えば上記警告に関しては Node 20 で動作する `actions/setup-java@v4` を利用していることが原因であることを伝えてくれています。

Node 20 は **2026年4月30日** に EOL を迎えました。現在は猶予期間として Node 20 でも動作しています。明確な時期はまだ明示されていませんが、いずれ Node 20 は完全に削除される予定のため、Node 24 への移行を推奨しています。

以下は各バージョンの生存期間の一覧です。GitHub 上で利用できなくなる時期とは必ずしも一致しません。

https://github.com/nodejs/Release/#end-of-life-releases

:::message alert
セルフホストしている場合は、Node 24 に対応した **2.327.1** 以降の Runner を利用する必要があります。
:::

https://github.com/actions/runner/releases/tag/v2.327.1

# 対応状況

:::message
筆者の利用しているモバイルアプリ関連の Actions が主になります
:::

各 Actions では Node 24 の対応が完了しています。以下はその対応済みバージョンの一覧です。表中のバージョンよりも新しいものがすでにリリースされている場合があります。

| Actions | Version | Release Note | Pull Request |
| -- | -- | -- | -- |
| checkout | v5.0.0 | https://github.com/actions/checkout/releases/tag/v5.0.0 | https://github.com/actions/checkout/pull/2226 |
| cache | v5.0.0 | https://github.com/actions/cache/releases/tag/v5.0.0 | https://github.com/actions/cache/pull/1630 |
| upload-artifact | v6.0.0 | https://github.com/actions/upload-artifact/releases/tag/v6.0.0 | https://github.com/actions/upload-artifact/pull/719 |
| setup-node | v5.0.0 | https://github.com/actions/setup-node/releases/tag/v5.0.0 | https://github.com/actions/setup-node/pull/1325 |
| setup-java | v5.0.0 | https://github.com/actions/setup-java/releases/tag/v5.0.0 | https://github.com/actions/setup-java/pull/888 |
| gradle/actions[^1] | v5.0.0 | https://github.com/gradle/actions/releases/tag/v5.0.0 | https://github.com/gradle/actions/pull/721 |

[^1]: gradle/actions に `setup-gradle` や `wrapper-validation` が含まれており、これらを統合するリポジトリとなっています。

# おわりに

Node 20 対応の記事を書いてから 2年半が経過していました。

https://zenn.dev/yumemi_inc/articles/github-actions-node-20

Node 24 の生存期間は **2028年4月30日**までです！

