---
title: "swift-java 環境構築"
emoji: "🌱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [swift,java]
published: true
---

Apple から Swift と Java を相互運用するためのライブラリ swift-java が公開されました。この記事では、リポジトリのサンプルを動かすための環境構築について説明します。

https://github.com/swiftlang/swift-java

Swift と Java の相互運用なので Swift と Java Development Kit（以下、JDK）の環境がそれぞれ必要になります。相互運用の手段がいくつか用意されていて、その手段ごとに必要とする JDK のバージョンが異なります。

| | JDK | Swift |
| - | - | - |
| JavaKit | 17+ | 6.0.x |
| swift-java jextract --mode=ffm | 24+ | 6.1 |
| swift-java jextract --mode=jni | 7+ | 6.1 |

そのため、 JDK のバージョン管理ツールとして SDKMAN! の導入を推奨します。Swift は、最新版の Xcode 16.4 に同梱される 6.1.2 で十分です。一時期は Swift 6.2 Development Snapshots を要求されていましたが、現時点では swift-java に既知の課題があって、6.2 をサポートできるまでは、それよりも前のバージョンの Swift が推奨されています。

https://github.com/swiftlang/swift-java/pull/266

## Java 環境構築

Java 環境構築のために SDKMAN! を導入します。

https://sdkman.io/install

以下は導入の一例です。

次のコマンドでインストールします。

```
curl -s "https://get.sdkman.io" | bash
```

次のコマンドでパスを通します。

```
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

JDK 17 をインストールします。JDK は様々な Distribution が用意されています[^java-distribution]。

[^java-distribution]: Java には、他の言語、プラットフォームにも見られる長期保守のリリース（以下、LTS）とそうではないリリース（非LTS）が存在します。一時期 swift-java では JDK 22 が要求されていた際に、JDK 22 は非LTSだったこともあり、唯一 Oracle だけが提供していることがありました。そのため、筆者の環境では Oracle の Distribution を選択しています。JDK 22 以外であれば、Oracle 以外からも提供されています。

```
sdk install java 17.0.12-oracle
```

JDK 17 を使うようにします。

```
sdk use java 17.0.12-oracle
```

Java のバージョンを確認します。

```
% java -version
java version "17.0.12" 2024-07-16 LTS
Java(TM) SE Runtime Environment (build 17.0.12+8-LTS-286)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.12+8-LTS-286, mixed mode, sharing)
```

JDK 24 を使う場合は、次のように切り替えます。

```
sdk install java 24-oracle
sdk use java 24-oracle
```

JDK 8 を使う場合は、次のように切り替えます[^jdk-8]。

```
sdk install java 8.0.442-zulu
sdk use java 8.0.442-zulu
```

[^jdk-8]: SDKMAN! では JDK 7 は提供がありません。Zulu から JDK 8 が提供されているのでこちらを利用するのが 7+ 以上の要求に対するミニマムバージョンになります。なお Amazon から提供されている JDK 8 ではサンプルを実行できませんでした。

## Swift 環境構築
swift-java に関しては macOS に限定されないため、他の環境向けに Swift をインストールすることでも相互運用可能です。

### macOS

macOS の場合は、Mac App Store から Xcode 16.4 をインストールもしくはアップデートするのがスムーズだと思います。

https://apps.apple.com/us/app/xcode/id497799835?mt=12

インストールまたはアップデート後に Xcode を一度起動し、Swift のバージョンを確認します。

```
% swift -version
swift-driver version: 1.120.5 Apple Swift version 6.1.2 (swiftlang-6.1.2.1.2 clang-1700.0.13.5)
Target: arm64-apple-macosx15.0
```
### Linux
https://www.swift.org/install/linux/

### Windows
https://www.swift.org/install/windows/

## サンプルコード

これで swift-java を動かす準備が整いました！
README に書いてあるコマンドを実行してサンプルコードを動かして見てください！
