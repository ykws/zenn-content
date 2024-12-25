---
title: "Upgrade Realm v4 to 10"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

Realm v4 から 10.17.0 に Upgrade しました

Gradle 8 に必要なのは 10.15.1 でした

今年に入って Android 毎年夏恒例の API Level の対応に備えて、
依存関係を整理

2024.8.31 までに 8.1.1 にアップデートして API Level 34 に対応する必要がありました

> Starting August 31 2024:
>
> New apps and app updates must target Android 14 (API level 34) to be submitted to Google Play

https://support.google.com/googleplay/android-developer/answer/11926878


API Level 34 の minimum AGP version が 8.1.1

https://developer.android.com/studio/releases#api-level-support

current は Gradle 7.4 でした。
Gradle を 8 系にアップデートするには、 Realm を 10.15.1 にアップデートする必要がありました

> This should be fixed with 10.15.1 which was just released. update your realm classpath in build.gradle to be :

https://stackoverflow.com/questions/76026649/build-failed-in-agp-8-0-failed-to-apply-plugin-realm-android-api-android-re

現状の Realm は v4 でした

2020年からの課題を 4年越しに対応することになりました。
4年も猶予を貰えたとも言えます。

10 系に一気に上げると問題の切り分けが難しかった


4.3.0 の時点で RealmObject がなくなっていた
勘違いだった


関連ライブラリのエラーもあった（省略）

UI スレッドで Realm の書き込み違反を一時的に許可して対応した


```java
allowWritesOnUiThread(true)
```

Java 8, 11 -> 17 

元々 Java 8 と 11 が混在

- SDK 33 -> 34
- Gradle 7.4.0 -> 8.3.0
- Realm 4.2.0 -> 10.17.0


整理すると、対応はシンプル

- findAllSorted("column_name") -> sort("column_name").findAll()
- findAllSorted("column_name").first(null) -> sort("column_name").findFirst()
- distinct("column_name") -> distinct("column_name").findAll()



