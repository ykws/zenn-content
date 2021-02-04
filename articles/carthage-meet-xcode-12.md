---
title: "Carthage Xcode12 との出会い"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Xcode,Carthage,Realm]
published: false
---

Carthage 0.37.0 では Xcode12 向けに XCFramework をビルド可能です。

既存のプロジェクトに対して以下のコマンドを実行します。

`$ carthage update --platform ios --use-xcframeworks`

`Carthage/Build` に `*.xcframework` が生成されます。

Copy link frameworks で `*.framework` で指定した箇所を `.xcframework` に置き換えます。

XCFramework ではコピーする必要はなくなるのでスクリプトから削除します。

## Realm で XCFramework が生成されない問題

realm-cocoa v10.5.2 に対して `--use-xcframeworks` のオプションを指定しても XCFramework が生成されません。

関連 Issue があり、 v10.2.0 から XCFramework に対応はしているとのことだが、生成できていない。

Carthage のビルド自体は成功し、今まで通り framework が生成されるので、そのまま Xcode12 でビルドできます。
