---
title: "Realm v4 から 10 に更新しました"
emoji: "🏆"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [contest2024, realm, android]
published: true
---

2024年 **「今年の最も大きなチャレンジ」** は、とある Android アプリを **Realm v4** から **10** に Upgrade したことです。

この Upgrade で最終的には、 9,000行ほどの差分になりました。

![](https://storage.googleapis.com/zenn-user-upload/6788731f0b3f-20241226.png =160x)

このアプリの Realm のバージョンは、正確には **v4.2.0** で、このバージョンがリリースされたのは **2017年11月17日** でした。

https://github.com/realm/realm-java/releases/tag/v4.2.0

今年に入って Android 毎年夏恒例の API Level を上げる対応に備えて依存関係を整理していました。
**8月31日**以降にアプリを更新したい場合は **API Level 34** に対応する必要がありました。

> Starting August 31 2024:
> - New apps and app updates must target Android 14 (API level 34) to be submitted to Google Play

https://support.google.com/googleplay/android-developer/answer/11926878

API Level 34 の minimum AGP version は **8.1.1** です。

![](https://storage.googleapis.com/zenn-user-upload/44116bdb1e3e-20241226.png)

https://developer.android.com/studio/releases#api-level-support

該当アプリでは Gradle **7.4.0** で、 **8系**にアップデートするには、 Realm を **10.15.1** にアップデートする必要がありました。

> This should be fixed with 10.15.1 which was just released. update your realm classpath in build.gradle to be :

https://stackoverflow.com/questions/76026649/build-failed-in-agp-8-0-failed-to-apply-plugin-realm-android-api-android-re

Realm を **10系**に一気に上げると問題の切り分けが難しく、段階的に上げながら課題を整理しました。 Realm そのものよりも Gradle を上げた影響かキャッシュが効かなくなって依存しているいくつかのライブラリ起因のビルドエラーに苦戦しました。今回の記事では、詳細は割愛します。

:::details 依存ライブラリ
- JCenter に依存しているライブラリは、暫定対応として JCenter から取得するようにしました。該当ライブラリは、現在メンテナンスされていないため、 JCenter 以外で配布されていないようでした
- 原因不明のビルドエラーを引き起こすライブラリは、よくよく確認するとそもそも利用していなくて、今回の対応で外すことにし、ビルドエラーを回避しました
:::

Realm の課題を整理すると、対応はシンプルでした。

```diff
- realm.findAllSorted("column_name")
+ realm.sort("column_name").findAll()
```

```diff
- realm.findAllSorted("column_name").first(null)
+ realm.sort("column_name").findFirst()
```

```diff
- realm.distinct("column_name")
+ realm.distinct("column_name").findAll()
```

UI スレッドで Realm の書き込み違反も数が多かったので、まとめて対応することは諦めて、一時的に許可し段階的に対応しました。

```java
allowWritesOnUiThread(true)
```

結果、元々 Java 8 と 11 が混在していましたが、 Java 17 に統一し、 API Level も要求に対応して今年の夏以降のアプリ更新もできるようになりました。

当初 Realm を v4 から 10 に変更すると、**300件**近いエラーがあって、対応はできないのではないかと心が折れそうになりました。問題を切り分けて、一気に解決することは諦めて、段階的な対応をすることで、何とか夏の更新に間に合ったのは、今年の最も大きなチャレンジとなりました。

Realm v10 へのアップデートは、このプロジェクトの Issue として **2020年10月** の時点で起票されており、 **約4年**の刻を経てクローズしました。
