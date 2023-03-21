---
title: "GitHub Actions でホステッドランナーを使う"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [github,github-actions,mac,ios]
published: true
publication_name: yumemi_inc
---

# はじめに

GitHub Actions を利用して iOS アプリの CI 環境を構築しようとすると、 CI 側に macOS 環境が必要になってきます。このとき **private リポジトリ**を対象とすると、 GitHub では、料金が高額になりやすいです。執筆時点では、 Linux の **10倍の速さで無料枠を消費**し、それを消化した後の **1分辺りの料金も 10倍**です。

![](https://storage.googleapis.com/zenn-user-upload/1644b114389a-20230318.png)
![](https://storage.googleapis.com/zenn-user-upload/8116a3dd9d98-20230318.png)

https://docs.github.com/ja/billing/managing-billing-for-github-actions/about-billing-for-github-actions

そこで登場するのが**セルフホステッドランナー**になります。

https://docs.github.com/ja/actions/hosting-your-own-runners/about-self-hosted-runners

これは自分で用意した(**self-hosted**)実行環境(**runner**)で CI を動かす仕組みです。つまり、 GitHub Actions で macOS 環境にかかるはずの**費用が 0** になります。

そのため、 GitHub private リポジトリで iOS アプリの CI 環境を構築する際の有力な選択肢になります。

:::message alert
public リポジトリでのセルフホステッドランナーは推奨されていません。 public リポジトリであれば、 macOS 環境であっても無料なので無理に採用する必要性もありません。
:::

> セルフホストランナーは、プライベートリポジトリでのみ利用することをおすすめします。 これは、ワークフロー内でコードを実行する pull request を作成することで、パブリック リポジトリのフォークによって、セルフホステッド ランナー マシン上で危険なコードが実行される可能性があるからです。

# 導入手順

リポジトリの Settings > Actions > Runner > New self-hosted runner から追加の手順が表示されるので、それに従えば良いです。コマンドの説明コメントと実行コマンドがリポジトリに合わせて展開されるので、導入環境でコピー＆ペーストしてコマンドを実行していきます。（導入手順のスクリーンショットは省略します。そのとき表示されている手順に従うのが正しいと思うため）

![](https://storage.googleapis.com/zenn-user-upload/95892b9e94f4-20230316.png)

手順中で重要なのは、 GitHub Actions の YAML に `runs-on: self-hosted` を指定する部分です。この記述によって、セルフホステッドランナーを利用して CI を実行するようになります。

## 導入対象

既成の public な iOS アプリのリポジトリを private にコピーして試しました。

https://github.com/ykws/XCTestExample

:::message alert
上記、リポジトリを private コピーしてセルフホステッドランナー設定前に GitHub Actions を実行してしまうと GitHub の macOS 環境で動作します
:::

## 環境構築

セルフホステッドする環境では、実行したい CI を処理できる必要があります。
今回導入を試したリポジトリでは、以下のコマンドでビルドとテストを実行します。

```
xcodebuild test -project XCTestExample.xcodeproj/ -scheme XCTestExample -sdk iphonesimulator -destination "platform=iOS Simulator,name=iPhone 14"
```

このコマンドでポイントとなるのは、次の2点です。

- xcodebuild
- destination

### xcodebuild

`xcodebuild` はコマンドラインで Xcode の GUI で行う操作が可能になります。ビルドとテストもできるようになります。以下は Apple が提供する xcodebuild のドキュメントです。

https://developer.apple.com/library/archive/technotes/tn2339/_index.html

初期状態では、 `xcodebuild` が有効ではないことがあります。その場合、実行したときに次のようなエラーになります。

```
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

有効な Xcode のディレクトリを指定する必要があります。コマンドラインから利用したい Xcode がインストールされているパスに合わせて、アプリ内の `Contents/Developer` を指定します。

```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

#### destination

`xcodebuild` のオプション `-destination` でターゲットを指定できます。このとき、有効な Simulator を指定する必要があります。無効な destination を指定すると次のようなエラーとなります。 `Available destinations` 以下に有効な destination を示してくれているので、この中から選んで指定します。

```
xcodebuild: error: Unable to find a destination matching the provided destination specifier:
		{ platform:iOS Simulator, OS:latest, name:iPhone 8 }

	The requested device could not be found because no available devices matched the request.

	Available destinations for the "XCTestExample" scheme:
                ...
		{ platform:iOS Simulator, id:AAA36B19-EFDD-4DB7-820B-4B8AEC75C1E9, OS:16.2, name:iPhone 14 }
		{ platform:iOS Simulator, id:9F9F7730-83D0-4842-9B0F-B9CCA441BC46, OS:16.2, name:iPhone 14 Plus }
		{ platform:iOS Simulator, id:FBC8230A-6816-4257-ABBC-3F2A0342E946, OS:16.2, name:iPhone 14 Pro }
		{ platform:iOS Simulator, id:6E1DD877-0283-41C8-87A4-EFFC7E75801E, OS:16.2, name:iPhone 14 Pro Max }
		{ platform:iOS Simulator, id:333054C3-F1DA-4978-9FEA-0CBBDF76B483, OS:16.2, name:iPhone SE (3rd generation) }
```

## 結果
- 快適です
- 実行速度は自環境のマシンスペックに依存します
  - 自分の環境では、 GitHub Actions で `7m 55s` かかっていたのが `1m 58s` になりました
- 自分の PC で runner のスクリプトが実行中でないと CI は動かないです

# 補足
## 設定解除

Runners の一覧から `Remove runner` が可能です。
private リポジトリから public に切り替える際は YAML と合わせて Runner も設定解除しましょう。

## 再設定

一つのランナーは一つのリポジトリに紐づきます（多分。誤って別のリポジトリをランナーに紐づけてしまい、違うリポジトリを紐づけようとしたらエラーになったため）
別のリポジトリで実行したい場合、ランナーを追加するか、ランナーの設定を解除して再設定が必要です。
ランナーの設定解除は以下の設定ファイルを削除することで可能です。

```
rm .credentials
rm .runner
```

その後 `./config.sh` から始まるコマンドを再実行します。

# おわりに

この内容をもって、ゆめみ iOS 研修に CI 環境を構築する課題を導入しようと思っていたのですが、 Session0 として、事前準備で取り組んでもらう内容としては重すぎるのと、結局 `runs-on: self-hosted` の設定を忘れてしまうと、 GitHub の macOS 環境で CI が動いてしまうことを回避できそうにないと感じたので、この内容での導入は難しいと感じています。

https://github.com/yumemi-inc/ios-training/pull/40

もしかしたら YAML をチェックして `runs-on: self-hosted` でなければ、 CI を止めるような仕組みで防御できるかもしれないので、別の機会にそちらも検証してみようと思いました。
