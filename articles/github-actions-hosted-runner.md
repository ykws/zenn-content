---
title: "GitHub Actions でホステッドランナーを使う"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [github-actions,mac]
published: false
---

# 動機

GitHub Actions を利用して iOS アプリの CI 環境を構築しようとすると、 macOS 環境が必要になってきます。このとき private リポジトリを対象とすると、料金が高額になりやすいです。執筆時点では、 Linux の **10倍の速さで無料枠を消費**し、 **1分辺りの料金も 10倍**です。

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

リポジトリの Settings > Actions > Runner > New self-hosted runner から追加の手順が表示されるので、それに従えば良いです。

![](https://storage.googleapis.com/zenn-user-upload/95892b9e94f4-20230316.png)


## 導入例

既成の public な iOS アプリのリポジトリを private にコピーして試しました。

https://github.com/ykws/XCTestExample

## 環境構築

セルフホステッドする環境で実行したい CI が処理できる必要があります。
今回導入を試したリポジトリでは、以下のコマンドでビルドとテストを実行します。

```
xcodebuild test -project XCTestExample.xcodeproj/ -scheme XCTestExample -sdk iphonesimulator -destination "platform=iOS Simulator,name=iPhone 14"
```

- `xcodebuild` でのビルドを有効な状態にします
  - `xcodebuild` が有効でない場合、以下のように Xcode の指定が必要です
  - `sudo xcode-select -s /Applications/Xcode.app/Contents/Developer`
- 有効な Simulator を指定します
  - 無効な Simulator を指定している場合は、有効な Simulator のリストが表示されるのでその中から選択します

# 特徴
- 実行速度は自環境のマシンスペックに依存します
  - 自分の環境では、 GitHub Actions で 7m 55s かかっていたのが 1m 58s になりました
- 自分の PC で runner のスクリプトが実行中でないと CI が動かないです

## 設定解除

Runners の一覧から `Remove runner` が可能です。

## 再設定

一つのランナーは一つのリポジトリに紐づきます（多分）
別のリポジトリで実行したい場合、ランナーを追加するか、ランナーの設定を解除して再設定が必要です。
ランナーの設定解除は以下の設定ファイルを削除することで可能です。

```
rm .credentials
rm .runner
```

## 再設定

一つのランナーは一つのリポジトリに紐づきます（多分）
別のリポジトリで実行したい場合、ランナーを追加するか、ランナーの設定を解除して再設定が必要です。
ランナーの設定解除は以下の設定ファイルを削除することで可能です。

```
rm .credentials
rm .runner
```

その後 `./config.sh` から始まるコマンドを実行すれば良いです。

**Enjoy CI!!**
