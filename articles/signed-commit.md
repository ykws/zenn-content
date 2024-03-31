---
title: "コミットに署名する"
emoji: "🪪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git,github]
published: false
---

> 「私はこれを記述した、そしてこの仕事の後ろには私が付いている」と。品質の証明として、あなたの署名が入っているべきなのです。[^signed]

[^signed]: アンドリュー・ハント／デビッド・トーマス著, 村上雅章訳(2000). 達人プログラマー ―システム開発の職人から名匠への道, ピアソン・エデュケーション.

# はじめに
コミットに署名しましょう

# ゴール
GitHub上でコミットに **Verified** のバッジがついた状態を目指します。
**Verified** をクリックすると、次のようにポップアップで署名の状態について確認できます。

![](https://storage.googleapis.com/zenn-user-upload/82ed7f16c123-20240324.png)

# 前提
Gitの初期設定ではコミットに署名は**されません**。
それでもコミットは可能ですが、匿名のコミットになります。
また署名には次のように段階があります。

1. 名前とメールアドレスが設定されている
2. コミットに署名されている
3. GitHub 上で有効な署名と認識される

ローカルでの作業と、リモートの作業がそれぞれ必要になります。
ローカルは macOS を、リモートは GitHub を例に説明します。
GitHub はドキュメントが充実しているのでリファレンスを示します。

# ローカルで作業する
## バージョンを確認する
記事執筆時のバージョンです。

| Name | Version | Releases |
| -- | -- | -- |
| Git | 2.44.0 | https://github.com/git/git/tags |
| Homebrew | 4.2.14 | https://github.com/Homebrew/brew/releases |
| Gnu Privacy Guard | 2.4.5 | https://github.com/gpg/gnupg/tags |
| PINEntry | 1.3.0 | https://github.com/gpg/pinentry/tags |

## ツールをインストールする
- Git
- Homebrew
- Gnu Privacy Guard
- PINEntry

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
Remote(GitHub) との認証に利用する SSH キーを生成します。

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

# リモートで作業する
Local の作業が完了したら次は Remote での作業です。

## SSH キーを追加する
Local で生成した SSH キーを GitHub に追加します。

https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

## メールアドレスを設定する
1つの GitHub アカウントに対して複数のメールアドレスを設定可能です。
参加している Organization からメールアドレスを発行されたら設定してリポジトリごとにメールアドレスを設定しましょう。

https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

## GPG キーを追加する
次のコマンドを実行し、ペーストボードにコピーされた内容を GPG キーとして追加します。

```
gpg --armor --export {key_id} | pbcopy
```

https://docs.github.com/ja/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

## 署名を強制する
管理者であれば Rulesets でこの署名を強制することができます。あなたが管理者であれば、ルールの適用を検討してみてください。管理者でない場合は、管理者とルールの適用を相談してみてください。

![](https://storage.googleapis.com/zenn-user-upload/2bbef50812ee-20240401.png)

https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule

# おわりに

冒頭でも引用した「達人プログラマー」からの言葉の続きを引用して終わります。この言葉に勇気づけられました。

> あなたの名前をコード中に見い出すことによって、みんな、それがきっちりと記述、テスト、ドキュメント化されたものであることを確認できるのです。それが本当のプロの仕事です。それが本当のプロによって記述されたものなのです。[^signed]

# 参考記事

https://zenn.dev/riscait/scraps/a41a82e7100360
https://qiita.com/s6n/items/bb869f740a53a3bf169e
https://qiita.com/ymatzki/items/814bd455a34609ef63df
