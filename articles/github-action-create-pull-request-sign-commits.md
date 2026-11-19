---
title: "GitHub Actions のコミットに署名する"
emoji: "🪪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [githubactions]
published: false
---

# はじめに
GitHub の branch protection rulesets では、branch を保護するために、コミットへの署名を必須にする設定[^1]が可能です。特に昨今セキュリティの観点からコミイットへの署名を必須にする設定は推奨されます。

[^1]: 署名付きコミットを要求する https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets#require-signed-commits

![](/images/github-actions-sign-commits-required.png)

GitHub Actions で Pull Request を作成する際に次の Actions を利用するとデフォルトではコミットに署名をしません。

https://github.com/marketplace/actions/create-pull-request

# コミットに署名する

冒頭の Actions のコミットに署名するオプションが用意されています。

```
sign-commits
```

`sign-commits` を `true` にすることで `GITHUB_TOKEN` を使って `github-actions[bot]` のコミットに署名します。

変更前は `Unverified` でしたが、変更後は `Verified` となりました。

| before | after|
| - | - |
| ![](/images/github-actions-sign-commits-before.png) | ![](/images/github-actions-sign-commits-after.png) |

# おわりに

GitHub Actions でのコミットに署名することも可能なので、積極的に branch protection rulesets での署名必須を活用していきましょう。

ただし、このように自動で署名をする仕組みがあると悪用もまた容易になることは懸念としてあります。過去にリーナスも指摘しています。

> Signing each commit is totally stupid. It just means that you automate it, and you make the signature worth less.[^2]

各コミットに署名するのは全く愚かなことです。それは単に自動化するだけで、署名の価値を下げてしまうだけです。今ここで行う署名は GitHub Workflow で実行されたことを証明できています。しかし、Workflow そのものに改竄があった場合に気付けません。自動化しているからこそ実行環境のシステムのステータスは注視しておく必要があります。

[^2]: GPG signing for git commit? https://web.archive.org/web/20210412010515/http://git.661346.n2.nabble.com/GPG-signing-for-git-commit-td2582986.html

# 関連
- [コミットに署名する](https://zenn.dev/yumemi_inc/articles/signed-commit)
- [コミットに署名するとはどういうことなのか](https://zenn.dev/yumemi_inc/articles/signing-commit-basics)
