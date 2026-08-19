---
title: "Zenn CLI を利用して記事を公開する"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [zenn,node,mac]
published: true
---

[Zenn公式](https://zenn.dev/zenn)アカウントから公式の記事が公開されており、十分な内容となってしますが、次の目的で記事として残しておきます。

- 全体の流れを俯瞰する
- 動作環境の記録

# 動作環境

| Name | Version |
| ---- | ------- |
| zenn-cli | v0.5.2 |
| macOS | 26.5 |
| node | v26.3.0 |
| npm/npx | 12.0.1 |

# 手順

1. GitHub にリポジトリを作成
2. Zenn と GitHub リポジトリを連携
3. [Node.js](https://nodejs.org/en/) をインストール
4. [Zenn CLI](https://github.com/zenn-dev/zenn-editor) をインストール
5. ローカルリポジトリの Zenn 用セットアップ
6. Zenn CLI で記事を作成
7. GitHub リポジトリを更新

## Issues

zenn-cli v0.1.157 - v0.1.158（それ以前のバージョンでも発生するかも）では DeprecationWarning が発生します。 zenn-cli が依存している markdown-it が原因で v14 以降で解消される見込みです。

```
(node:30404) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
```

https://github.com/zenn-dev/zenn-editor/issues/476

## コンテンツ管理
次のリポジトリで Zenn コンテンツを管理しています。

https://github.com/ykws/zenn-content

# 参考記事
詳細は公式アカウントの次の記事を順に読むのが良いです。

https://zenn.dev/zenn/articles/connect-to-github
https://zenn.dev/zenn/articles/install-zenn-cli
https://zenn.dev/zenn/articles/zenn-cli-guide
