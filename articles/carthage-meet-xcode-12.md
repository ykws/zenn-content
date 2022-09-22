---
title: "Xcode12 で Carthage を利用する"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [iOS,Xcode,Carthage,Realm]
published: true
---

[Carthage 0.37.0](https://github.com/Carthage/Carthage/releases/tag/0.37.0) では Xcode12 向けに XCFramework をビルド可能です。

# Xcode12 で Carthage を利用するには

既存のプロジェクトに対して以下のコマンドを実行します。

```
$ carthage bootstrap --platform ios --use-xcframeworks
```

1. `Carthage/Build` に `*.xcframework` が生成されます。
2. Link Binary with Libraries で `*.framework` を指定していた箇所を `*.xcframework` に置き換えます。
3. XCFramework では `*.framework` コピーする必要がなくなるので `copy-frameworks` の Input Files から削除します。

## Realm で XCFramework が生成されない問題

realm-cocoa v10.5.1 に対して `--use-xcframeworks` のオプションを指定しても XCFramework が生成されません。

- [関連 Issue](https://github.com/realm/realm-cocoa/issues/7031)

[v10.2.0](https://github.com/realm/realm-cocoa/releases/tag/v10.2.0) から XCFramework に対応しているとのことですが、生成できていません。

Carthage のビルド自体は成功し、今まで通り `*.framework` が生成されるので、特に設定は変更せずにそのまま Xcode12 でビルドできます。

## Carthage が Xcode12 に対して抱えていた問題と XCFramework について深く知る

* [[日本語訳] Carthage issues, Xcode 12, XCFrameworks, Apple Silicon, etc.](https://zeero.medium.com/%E6%97%A5%E6%9C%AC%E8%AA%9E%E8%A8%B3-carthage-issues-xcode-12-xcframeworks-apple-silicon-etc-fa1932769ad3)
* [[原文] Carthage issues, Xcode 12, XCFrameworks, Apple Silicon, etc.](https://medium.com/@quentinfasquel/carthage-issues-xcode-12-xcframeworks-apple-silicon-etc-1c60d8635dbc)
