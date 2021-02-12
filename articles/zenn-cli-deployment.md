---
title: "Zenn CLI を利用して記事を公開する"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [zenn]
published: true
---

[Zenn公式](https://zenn.dev/zenn)アカウントから公式の記事が公開されており、十分な内容となってしますが、次の目的で記事として残しておきます。

- 全体の流れを俯瞰する
- 動作環境の記録

# 動作環境

| Name | Version |
| ---- | ------- |
| zenn-cli | v0.1.72 |
| macOS | 11.2 |
| node | v15.8.0 |
| npm/npx | 7.5.3 |
| n | 7.0.1 |

# 手順

1. GitHub にリポジトリを作成
2. Zenn と GitHub リポジトリを連携
3. Zenn CLI をインストール
4. ローカルリポジトリの Zenn 用セットアップ
5. Zenn CLI で記事を作成
6. GitHub リポジトリを更新

## Zenn コンテンツ管理リポジトリ
自分の場合は次のリポジトリで Zenn コンテンツを管理するようにしてみました。

[zenn-content](https://github.com/ykws/zenn-content)


# 参考記事
詳細は公式アカウントの次の記事を順に読むのが良いです。

- [GitHubリポジトリでZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)
- [Zenn CLIをインストールする](https://zenn.dev/zenn/articles/install-zenn-cli)
- [Zenn CLIを使ってコンテンツを作成する](https://zenn.dev/zenn/articles/zenn-cli-guide)
