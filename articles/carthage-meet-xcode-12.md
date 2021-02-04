---
title: "Xcode12 で Carthage を利用する"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [iOS,Xcode,Carthage,Realm]
published: false
---

[Carthage 0.37.0](https://github.com/Carthage/Carthage/releases/tag/0.37.0) では Xcode12 向けに XCFramework をビルド可能です。

既存のプロジェクトに対して以下のコマンドを実行します。

```
$ carthage update --platform ios --use-xcframeworks
```

1. `Carthage/Build` に `*.xcframework` が生成されます。
2. Link Binary with Libraries で `*.framework` を指定していた箇所を `*.xcframework` に置き換えます。
3. XCFramework では `*.framework` コピーする必要がなくなるので `copy-frameworks` の Input Files から削除します。

## Realm で XCFramework が生成されない問題

realm-cocoa v10.5.1 に対して `--use-xcframeworks` のオプションを指定しても XCFramework が生成されません。

- [関連 Issue](https://github.com/realm/realm-cocoa/issues/7031)

[v10.2.0](https://github.com/realm/realm-cocoa/releases/tag/v10.2.0) から XCFramework に対応しているとのことですが、生成できていません。

Carthage のビルド自体は成功し、今まで通り `*.framework` が生成されるので、特に設定は変更せずにそのまま Xcode12 でビルドできます。
