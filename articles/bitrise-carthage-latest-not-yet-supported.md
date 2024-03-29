---
title: "Bitrise では Carthage 0.37.0 をサポートしていませんでした"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [iOS,Xcode,Carthage,Bitrise]
published: true
publication_name: yumemi_inc
---

[前の記事](https://zenn.dev/ykws/articles/carthage-meet-xcode-12)で Carthage 0.37.0 では Xcode12 向けに XCFramework をビルド可能と書きましたが、当時の Bitrise では **0.36.0** が最新としてバンドルされているため、しばらくの間 XCFramework が利用できませんでした。

現時点では **0.38.0** をサポートしており、利用可能です。
正式にサポートされたのは以下のコミットのようです。

https://github.com/bitrise-io/bitrise.io/commit/754e9522a055c084b65aec927c5ac7ef86b3d3a2

以下は備忘録として残しておきます。

Bitrise Feature Requests に Update Carthage 0.37.0 があるので、投票をしました。同じように困っている人は投票すると早めに対応されるかもしれません。

https://discuss.bitrise.io/t/update-carthage-to-0-37-0/15648

Carthage step for iOS/Mac も Bitrase の Carthage に依存しているので同様です。

https://github.com/bitrise-steplib/steps-carthage/issues/64

Bitrise Script で `brew update && brew upgrade carthage` が有効なこともあったようですが、現状は 0.37.0 がインストールできることを確認できていません。

https://twitter.com/_mono/status/1050256628371488768

よって、 Bitrise で Carthage を利用するには既知の通り Workaround を利用した対応が必要になります。

https://github.com/Carthage/Carthage/blob/master/Documentation/Xcode12Workaround.md

具体的な Bitrise の設定は次の記事がわかりやすいです。

https://zenn.dev/tihimsm/articles/d2e85982f8e9114e0814
