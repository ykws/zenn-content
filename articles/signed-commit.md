---
title: "コミットに署名する"
emoji: "🪪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git,github]
published: true
publication_name: yumemi_inc
---

> 「私はこれを記述した、そしてこの仕事の後ろには私が付いている」と。品質の証明として、あなたの署名が入っているべきなのです。[^signed]

[^signed]: アンドリュー・ハント／デビッド・トーマス著, 村上雅章訳(2000). 達人プログラマー ―システム開発の職人から名匠への道, ピアソン・エデュケーション.

# はじめに
コミットに署名しましょう

## ゴール
GitHub 上でコミットに **Verified** のバッジがついた状態を目指します。**Verified** をクリックすると、次のようにポップアップで署名の状態について確認できます。

![](https://storage.googleapis.com/zenn-user-upload/82ed7f16c123-20240324.png)

## 前提
Git の初期設定ではコミットに署名は**されません**。それでもコミットは可能ですが、匿名のコミットになります。また署名には次のように段階があります。

1. 名前とメールアドレスが設定されている
2. コミットに署名されている
3. ホスト上で有効な署名として認識されている

ローカルで署名できるようにする作業と、ホスト側で署名を有効化する作業がそれぞれ必要になります。ローカルは macOS を、ホスト側は GitHub を例に説明します。GitHub はドキュメントが充実しているのでリファレンスを示します。

:::message
もし手元の環境で部分的にすでに対応済みの項目は読み飛ばして進めてください。例えば、すでに SSH キーを生成済みであれば、新しく生成し直す必要はありません。
:::

# 署名できるようにする
## ツールをインストールする
記事執筆時のバージョンです。

:::message alert
xz 問題があったので Homebrew **4.2.15** 以上の利用を強く推奨します
:::

:::details xz 問題
xz v5.6.0/5.6.1 にバックドアが仕込まれてしまい、 Homebrew 4.2.14 以前では該当バージョンがインストールされる可能性があります。いつからか調べていないですが、この記事執筆時点では **4.2.14** と記載しており、筆者の環境にも xz 5.6.1 がインストールされてしまっていました。
Homebrew 4.2.15 では xz をダウングレードして 5.4.6 がインストールされるようになっているため、安全です。詳しくは下記で議論されています。

https://github.com/orgs/Homebrew/discussions/5243
:::

| Name | Version | Releases |
| -- | -- | -- |
| Git | 2.44.0 | https://github.com/git/git/tags |
| Homebrew | 4.2.15 | https://github.com/Homebrew/brew/releases |
| Gnu Privacy Guard | 2.4.5 | https://github.com/gpg/gnupg/tags |
| PINEntry | 1.3.0 | https://github.com/gpg/pinentry/tags |

### Git　をインストールする
macOS では最初から Git がインストール済みです。

### Homebrew をインストールする
次のコマンドでインストールできます。

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

https://brew.sh/

### Gnu Privacy Guard & PINEntry をインストールする
Homebrew をインストールすると `brew` コマンドが使えるようになるので、次のコマンドでインストールできます。

```
brew install gpg pinentry
```

## SSH キーを生成する
GitHub との認証に利用する SSH キーを生成します。

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

## 名前を設定する
コミットに記録する名前を設定します。

```
git config --global user.name "Mona Lisa"
```

https://docs.github.com/ja/get-started/getting-started-with-git/setting-your-username-in-git

## メールアドレスを設定する
コミットに記録するメールアドレスを設定します。

```
git config --global user.email "YOUR_EMAIL"
```

https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

## GPG キーを生成する

```
gpg --full-gen-key
```

`key_id` は生成結果の `sec` の暗号方式の `/` 以下の文字列です。

```
git config --global user.signingkey {key_id}
git config --global commit.gpgsign true
```

実際のコミットが GPG 署名されているか確認する

```
git log --show-signature -1
```

`gpg: Good signature from ...` と表示されていればOK（... の部分には、鍵作成時に入力した名前とメールアドレスが表示されます）
未署名だと、 `gpg:` の部分が表示されません。

https://docs.github.com/ja/authentication/managing-commit-signature-verification/generating-a-new-gpg-key

# ホスト側で署名を有効化する

## SSH キーを追加する
ローカルで生成した SSH キーを GitHub に追加します。

https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

## メールアドレスを設定する
1つの GitHub アカウントに対して複数のメールアドレスを設定可能です。
参加している Organization からメールアドレスを発行されたら設定してリポジトリごとにメールアドレスを設定しましょう。

https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

## GPG キーを追加する
次のコマンドを実行し、ペーストボードにコピーされた内容を GitHub に GPG キーとして追加します。

```
gpg --armor --export {key_id} | pbcopy
```

https://docs.github.com/ja/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

## 署名を強制する
管理者であれば Rulesets でこの署名を強制することができます。あなたが管理者であれば、ルールの適用を検討してみてください。管理者でない場合は、管理者とルールの適用を相談してみてください。

![](https://storage.googleapis.com/zenn-user-upload/2bbef50812ee-20240401.png)

https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule

# おわりに

冒頭で引用した「達人プログラマー」からの言葉の続きを引用して終わります。当時この言葉にとても勇気づけられました。

> あなたの名前をコード中に見い出すことによって、みんな、それがきっちりと記述、テスト、ドキュメント化されたものであることを確認できるのです。それが本当のプロの仕事です。それが本当のプロによって記述されたものなのです。[^signed]

# 参考記事

https://zenn.dev/riscait/scraps/a41a82e7100360
https://qiita.com/s6n/items/bb869f740a53a3bf169e
https://qiita.com/ymatzki/items/814bd455a34609ef63df
