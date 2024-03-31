---
title: "コミットに署名する"
emoji: "🪪"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git,github]
published: false
---

# はじめに
コミットに署名しましょう

達人プログラマからの署名しようの項を引用

# ゴール
GitHub上でコミットにVerifiedのバッジがついた状態を目指します。
Verified をクリックすると、次のようにポップアップで署名の状態について確認できます。

![](https://storage.googleapis.com/zenn-user-upload/82ed7f16c123-20240324.png)

# 前提
Gitの初期設定ではコミットに署名はされません
それでもコミットは可能ですが、匿名のコミットになります
また署名には段階があります

1. 名前とメールアドレスが設定されている
2. コミットに署名されている

さらにGitHubにメールアドレスとキーを設定することでVerifiedと認証された状態にできます

# 作業
Local での作業と、リモートの作業がそれぞれ必要になります。
Local は macOS を、リモートは GitHub を例に説明します。

## Local

### バージョン確認

| Name | Version | Releases |
| -- | -- | -- |
| Git | 2.44.0 | https://github.com/git/git/tags |
| Homebrew | 4.2.14 | https://github.com/Homebrew/brew/releases |
| Gnu Privacy Guard | 2.4.5 | https://github.com/gpg/gnupg/tags |
| PINEntry | 1.3.0 | https://github.com/gpg/pinentry/tags |

### インストール
- Git
- Homebrew
- GPG

#### Git
macOS では最初から Git がインストール済みです。

#### Homebrew
以下のサイトを参考に次のコマンドでインストールできます。

https://brew.sh/

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Gnu Privacy Guard & PINEntry

```
brew install gpg pinentry
```

### SSHキー生成

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent


### 名前設定

```
git config --global user.name "Mona Lisa"
```

https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git

### メールアドレス設定

```
git config --global user.email "YOUR_EMAIL"
```

https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

### GPG署名設定

```
gpg --full-gen-key
```

`key_id` は生成結果の `sec` の暗号方式の `/` 以下の文字列です。

```
gpg --armor --export {key_id} | pbcopy
```

ペーストボードにコピーした内容を GitHub GPG として登録します。

https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

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

## Remote(GitHub)

### キー

https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

### メールアドレス

1つの GitHub アカウントに対して複数のメールアドレスを登録可能です。
参加している Organization からメールアドレスを発行されたら追加してリポジトリごとにメールアドレスを設定しましょう。

https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address

### Verified

https://docs.github.com/ja/authentication/troubleshooting-commit-signature-verification/checking-your-commit-and-tag-signature-verification-status

# おわりに

