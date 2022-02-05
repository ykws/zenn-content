---
title: "GoogleAuthenticatorをソースからビルドしてみよう"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [onetimepassword, android, bazel]
published: true
---

# はじめに
二段階認証のパスコードに Google Authenticator を利用している人も多いと思います。

https://apps.apple.com/us/app/google-authenticator/id388497605
https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2

Google Authenticator for Android のオープンソース版があります。

:::message alert
2020年6月9日が最終更新日でアーカイブなっており、現在はメンテナンスされていません。
:::

https://github.com/google/google-authenticator-android

今回はこのオープンソース版を試してみます。
結論として、実機で動作確認までできましたが、問題があって未解決の状態です。

README に書いてある通りですが、順に見ていきます。

# 動作確認環境
- Mac mini M1 2020
- Android Studio Arctic Fox 2020.3.1 Pathc 3
- Homebrew 3.2.16
- Bazel 4.2.1-homebrew

# 環境構築
## Android Studio
Download Android Studio ->  Mac with Apple chip から M1対応版をダウンロードできます。
https://developer.android.com/studio

## Bazel
M1対応済みを、 Homebrew 経由でインストールできます。
https://docs.bazel.build/versions/main/install-os-x.html#install-on-mac-os-x-homebrew

:::message
オープンソース版は Android Studio のプロジェクト形式ではないため、 Bazel が必須になっています。
:::

# ソースを取得 
GitHub からソースを取得します。

# パスを通す
ANDROID_HOME として Android SDK のパスを通します。
以下のようにパスを認識していれば大丈夫です。

```
% echo $ANDROID_HOME
/Users/ykws/Library/Android/sdk
```

# 実機インストール
Bazel を利用してビルドして、実機にインストールします。

:::message
注意すべきは `//` はよくあるコメントではなく、パラメータとしてターミナルに入力します。
:::

以下、 `//` 以下も含めて全て入力するのが正しいです。

```
% bazel mobile-install //java/com/google/android/apps/authenticator
```

# 動作確認
実機インストール後にフリーの OTP QR 生成サイトで動作確認します。

https://freeotp.github.io/qrcode.html

バーコードをスキャンからカメラを起動し、サイトの QR コードを読み取るとアカウントが追加され、ワンタイムパスワードを生成できるようになります。

![](https://storage.googleapis.com/zenn-user-upload/3164b7f895cfb84285dd97ee.png =250x)

# 問題
サイト上で生成するパラメータ（桁数、SHA、Counter or Timeout）を変更しても、生成されるパスコードが全く同じなので、アプリ側でパラメータを調整可能になっていない可能性があります。

