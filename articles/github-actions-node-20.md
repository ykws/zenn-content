---
title: "GitHub Actions Node 20 対応状況"
emoji: "👷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [githubactions, nodejs]
published: false
---

# はじめに
GitHub Actions で Node 16 を利用していると次のような警告が表示されています。

> Report dependency differences
Node.js 16 actions are deprecated. Please update the following actions to use Node.js 20: actions/setup-java@v3. For more information see: https://github.blog/changelog/2023-09-22-github-actions-transitioning-from-node-16-to-node-20/.

Node 16 のメンテナンスは 2023年9月11日で終了しています。以下は各バージョンの生存期間の一覧です。

https://github.com/nodejs/Release/#end-of-life-releases

# 対応状況

:::message
モバイルアプリ関連の Actions が主になります
:::

各 Actions では Node 20 の対応が進んでいます。以下はその対応状況です。

| 名前 | 対応バージョン | Release Note | Pull Request |
| -- | -- | -- | -- |
| checkout | v4.0.0 | https://github.com/actions/checkout/releases/tag/v4.0.0 | https://github.com/actions/checkout/pull/1436 |
| cache | v4.0.0 | https://github.com/actions/cache/releases/tag/v4.0.0 | https://github.com/actions/cache/pull/1284 |
| setup-node | v4.0.1 | https://github.com/actions/setup-node/releases/tag/v4.0.1 | https://github.com/actions/setup-node/pull/889 |
| setup-java | v4.0.0 | https://github.com/actions/setup-java/releases/tag/v4.0.0 | https://github.com/actions/setup-java/pull/558 |
| gradle-build-action | v3.0.0 | https://github.com/gradle/gradle-build-action/releases/tag/v3.0.0 | https://github.com/gradle/gradle-build-action/issues/946 |
| auto-assign-action | v1.2.6 | https://github.com/kentaro-m/auto-assign-action/releases/tag/v1.2.6 | https://github.com/kentaro-m/auto-assign-action/pull/158 |
| review-assign-action | - | - | https://github.com/hkusu/review-assign-action/pull/32 |

# おわりに

Node 20 の生存期間は 2026年4月30日までです！