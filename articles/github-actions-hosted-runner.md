---
title: "GitHub Actions でホステッドランナーを使う"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [github-actions]
published: false
---

# 動機

GitHub Actions を利用して iOS アプリの CI 環境を構築しようとすると、 macOS 環境が必要になってきます。このとき private リポジトリを対象とすると、料金が高額になりやすいです。執筆時点では、 Linux の 10倍の速さで無料枠を消費し、 1分辺りの料金も 10倍です。

https://docs.github.com/ja/billing/managing-billing-for-github-actions/about-billing-for-github-actions

そこで登場するのがセルフホステッドランナーになります。

https://docs.github.com/ja/actions/hosting-your-own-runners/about-self-hosted-runners

これは自分で用意した(self-hosted)実行環境(runner)で CI を動かすことができるようになります。つまり、 GitHub Actions で macOS 環境にかかるはずの費用が 0 になります。

というわけで、 GitHub private リポジトリで iOS アプリの CI 環境を構築する際の有力な選択肢になります。

## 注意事項

public リポジトリでのセルフホステッドランナーは推奨されていません。 public リポジトリであれば、 macOS 環境であっても無料なので無理に採用する必要性もありません。

> セルフホストランナーは、プライベートリポジトリでのみ利用することをおすすめします。 これは、ワークフロー内でコードを実行する pull request を作成することで、パブリック リポジトリのフォークによって、セルフホステッド ランナー マシン上で危険なコードが実行される可能性があるからです。

# 導入手順

リポジトリの Settings > Actions > Runner > New self-hosted runner から追加の手順が表示されるので、それに従えば良いです。

## 導入例

既成の public な iOS アプリのリポジトリを private にコピーして試しました。

https://github.com/ykws/XCTestExample

## 環境構築

- `xcodebuild` でのビルドを有効な状態にします
  - `xcodebuild` が有効でない場合、以下のように Xcode の指定が必要です
  - `sudo xcode-select -s /Applications/Xcode.app/Contents/Developer`
- 有効な Simulator を指定します
  - 無効な Simulator を指定している場合は、有効な Simulator のリストが表示されるのでその中から選択します

# 特徴
- 自環境のマシンスペックに依存します
  - 自分の環境では、 GitHub よりも速かったです
- 自分の PC でスクリプトが実行中でないと CI が動かないです

## 設定解除

Runners の一覧から Remove runner が可能です。

## 再設定

一つのランナーは一つのリポジトリに紐づきます（多分）
別のリポジトリで実行したい場合、ランナーを追加するか、ランナーの設定を解除して再設定が必要です。
ランナーの設定解除は以下の設定ファイルを削除することで可能です。

```
rm .credentials
rm .runner
```

その後 `c が可能です。

## 再設定

一つのランナーは一つのリポジトリに紐づきます（多分）
別のリポジトリで実行したい場合、ランナーを追加するか、ランナーの設定を解除して再設定が必要です。
ランナーの設定解除は以下の設定ファイルを削除することで可能です。

```
rm .credentials
rm .runner
```

その後 `./config.sh` から始まるコマンドを実行すれば良いです。

Enjoy CI!

