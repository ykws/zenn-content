---
title: "依存関係とどう向き合っていくか"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [kotlin,kotlinmultiplatfor]
published: false
---

> プロジェクトはゆっくりと、そして容赦なく、手に負えないものとなっていくのです。ソフトウェアにおける惨事のほとんどは、たいていのプロジェクトでは気づかずに通り過ぎてしまうような、気づくには小さすぎるとも思えるものから始まるのです。
>
> アンドリュー・ハント／デビッド・トーマス著, 村上雅章訳（2020）. 達人プログラマーーシステム開発の職人から名匠への道ー. ピアソン p.8

> プロジェクトはゆっくりと、そして容赦なく、手に負えないものとなっていくのです。ソフトウェアにおける惨事のほとんどは、たいていのプロジェクトでは気づかずに通り過ぎてしまうような、気づくには小さすぎるとも思えるものから始まるのです。
>
> アンドリュー・ハント／デビッド・トーマス著, 村上雅章訳（2020）. 達人プログラマーーシステム開発の職人から名匠への道ー. ピアソン p.8

本記事は Zenn にも同等の内容を公開しています。出版後に加筆、修正がある場合は Zenn の記事で確認できます。 Zenn ではコメントも開放しているのでフィードバックも歓迎しています。

https://zenn.dev/yumemi_inc/articles/dependency-panic

# はじめに

現代のモバイルアプリケーション開発は様々な要因の依存関係と向き合っていく必要があります。

昔のことを思い出すと今から 20年前のモバイルアプリは Java ME をベースに開発をしていました。アプリケーションのサイズ制限が 100KB ということもあって、依存するものは極力少なくするために、 Java SDK そのものもモバイル向けに軽量にしていました。 ME は Micro Edition の略称です。現在も組み込み系で、容量に制限のある環境では、 Java ME を採用しているプロダクトはありますが、モバイル環境においては、スマートフォンの登場により大きく変化し、アプリケーションのサイズ制限を気にすることなく、ライブラリを活用できるようになりました。

さて、一方で課題となるのは、アプリケーションが依存するライブラリ同士の依存関係があります。一方のバージョンを上げるためには、別のライブラリのバージョンも上げる必要があり、またのその別のライブラリが依存するさらにまた別のライブラリと、依存関係が課題が連鎖することもあり得ます。

さらにライブラリ以外にもモバイルアプリ構成の核となる OSと言語も毎年大きなアップデートがあり、小さな更新は一年の間にも複数回発生します。この OS と言語をベースとしてライブラリ群なので、これらの変更にもライブラリは影響を受けます。

特にモバイルアプリが対応する OS は下限をサポートするほどにより多くのユーザが利用可能になる一方で、サポートのために最新 OS の機能を取り込みにくいというジレンマが発生します。

言語仕様も同様です。 OS と言語は密接に関連するため、特定の OS と言語の組み合わせにも制限が生じる可能性もあります。

https://www.oracle.com/java/technologies/javameoverview.html
https://ja.wikipedia.org/wiki/I%E3%82%A2%E3%83%97%E3%83%AA
https://ja.wikipedia.org/wiki/S!%E3%82%A2%E3%83%97%E3%83%AA

# 事例紹介

## UUID

先日 Kotlin Multiplatform でのモバイルアプリ開発において、 kotlinx-serialization-json を 1.7.1 から 1.7.2 にアップデートとした時のことです。ビルドは通ったのですが、アプリを起動するとクラッシュしてしまいました。

iOS/Android でそれぞれ次のような実行時エラーが発生しました。

iOS

```swift
kotlin.native.internal.IrLinkageError: Reference to class 'Uuid' can not be evaluated: No class found for symbol 'kotlin.uuid/Uuid|null[0]'
```

Android

```kotlin
kotlinx.serialization.SerializationException: Serializer for class 'User' is not found.
```

このエラーは、 kotlin-serialization-json 1.7.2 から標準ライブラリの kotlin.uuid に依存するようになったにもかからわず、アプリ内部ではサードパーティの benasher44/uuid を使用しているため、 kotlinx-serizalization-json 側で kotlin.uuid/Uuid のクラス解決ができないことが原因でした。

先ほどの kotlin-serizalization-json 1.7.2 のリリースノートの抜粋です。

![](https://storage.googleapis.com/zenn-user-upload/03ceaef95f03-20241004.png)

https://github.com/Kotlin/kotlinx.serialization/releases/tag/v1.7.2

注目すべきは **It uses Kotlin 2.0.20 by default.** です。このように依存するバージョンが defalut となるのは注意するキーワードとなります。

さらに Kotlin 2.0.20 から導入された kotlin.uuid.Uuid を導入するというのも重要な変更点となりました。

![](https://storage.googleapis.com/zenn-user-upload/4daed4833b9d-20241004.png)

Kotlin 2.0.20 のリリースノートで標準ライブラリとして UUID が導入されたことが紹介されていました。著者は当時このリリースノートを先に目にしていたはずなのですが、ライブラリを更新する時点ではすっかり忘れていました。

![](https://storage.googleapis.com/zenn-user-upload/be2a6d40a6be-20241004.png)

https://kotlinlang.org/docs/whatsnew2020.html#standard-library

今回発生しているクラッシュは、サードパーティの UUID ライブラリから標準ライブラリの UUID を利用することで解決できます。ただし、発生時点では SKIE が Kotlin 2.0.20 に未対応でした。Swift から KMP を扱いやすくライブラリで KMP の開発で重宝していました。

https://github.com/touchlab/SKIE/issues/95

この SKIE の対応がブロック要因となって UUID に依存するライブラリ群の更新を見合わせる判断を当時はすることになりました。

（執筆時点では、 SKIE は Kotlin 2.0.20 に対応した 0.9.0 がリリースされています）

https://skie.touchlab.co/changelog/0.9.0

## Xcode 16

KMP で iOS アプリ開発に Xcode を利用していると、 Kotlin 2.0.20 が Xcode 16 に未対応のため、 Xcode 15 を利用し続ける必要があります。

https://youtrack.jetbrains.com/issue/KT-69093

この問題は Kotlin 2.0.21-RC で対応されます。

https://github.com/JetBrains/kotlin/releases/tag/v2.0.21-RC

RC でも Kotlin のバージョンを上げることができれば解消可能なのですが、ここでも SKIE の Kotlin への追従状況が対応へのブロック要因になってしまいます。

# 向き合い方
紹介した事例のように何か問題が起きたり、あるいはその兆候を感じた時にどのように向き合っていくか、筆者の取り組みを紹介する形で共有します。

まずは、ライブラリへの依存の代替手段がないか検討します。前述の UUID のように元々言語仕様としてサポートしていなかったため、サードパーティのライブラリを利用していたが、言語の更新により標準ライブラリが利用可能な場合は、サードパーティのライブラリから標準ライブラリに切り替える良い機会になると思います。
代替手段がなく、あるいは現時点では選択困難な状況であり、対象が OSS であれば、 Issues を是非とも活用したいです。 OSS ではなくバイナリだけ配布し、 Issues を活用しているライブラリもあります。

- Issues を確認する
- Issue を作成する
- Issue にコメントする
- Issue に対応する

Issues がオープンでなくても、あるいは併用して、 Slack や Track など課題を共有、管理できるツールが提供されていることもあり、適宜読み替えてください。

## Issues を確認する

Issues を検索してみましょう。エラーメッセージや発生バージョンなどを条件に検索することで、先に他の人がレポートを上げていることがあります。

例えば、 SKIE が Kotlin 2.0.20 に対応していない Issue を探すと下記が見つかります。

https://github.com/touchlab/SKIE/issues/95

## Issue を作成する

十分に検索してヒットしない場合は、 Issue を作成しましょう。このとき、自分が検索した時を思い出し、エラーメッセージや発生バージョン、付帯条件など検索性を意識して作成すると、重複した Issue の乱立を防げ、メンテナーに対しても問題を正確に伝えることができます。

MagicPod 導入時に発生した問題が検索しても情報が得られず、 Issue を作成したことがあります。

https://github.com/Magic-Pod/magicpod-api-client/issues/15

## Issue にコメントする

Issue を読んで問題が未解決の場合は、内容を確認して、自分と同条件であれば、スタンプや same me とコメントするのも重要な付加情報と考えています。完全に状況が一致していない場合は、発生条件や結果の差異についてもコメントしてみましょう。解析の助けや、検索性の向上に繋がります。

Koin 4.0.0 でも標準ライブラリの UUID に切り替えたことで、 kotlinx-serialization-json 1.7.2 で起きていたことと同様の問題が発生していました。そこで得た情報をコメントで共有しました。

https://github.com/InsertKoinIO/koin/issues/2003#issuecomment-2378196965

## Issue に対応する

Issue の内容が対応可能であれば、 Pull Request を送りましょう。CI が用意されていることが多いので、これを積極的に活用して、安心して挑戦できる環境が用意されています。

Kable にもサードパーティの UUID の依存があって、Kotlin 2.0.20 対応のために標準ライブラリへの切り替えが必要でした。それまで複数の関連ライブラリで標準ライブラリへの切り替えを見てきたので挑戦してみました。

https://github.com/JuulLabs/kable/pull/758

# おわりに

今回紹介した内容が、これから先、これを読んでいるあなたが依存関係とどう向き合っていくかの判断材料になれば嬉しいです。現代のプログラミングは変化の速度の速い領域に位置していて、どんなに丁寧に依存関係と向き合っていても、混乱の生じる場面は避けられないこともあります。そんな場面に遭遇したら機会点と捉えて、課題を共有して共に向き合っていくことでより良いサイクルを作っていきたいです。
