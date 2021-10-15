---
title: "GoogleAuthenticatorを触ってみよう"
emoji: "⛳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [onetimepassword, android, bazel]
published: false
---

二段階認証のパスコード生成に Google Authenticator を利用している人も多いと思います。

## iOS
https://apps.apple.com/us/app/google-authenticator/id388497605

## Android
https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2

Google Authenticator for Android のオープンソース版があります。
現在はメンテナンスされていません。

https://github.com/google/google-authenticator-android

今回はこのオープンソース版を試してみます。

README に書いてある通りです。

まずは GitHub からソースを取得します。

ANDROID_HOME は定義済みでした。

bazel を利用してビルドします。

bazel の M1 対応は済んでおり、 Homebrew 経由でインストールすると問題なく動かせます。
https://docs.bazel.build/versions/main/install-os-x.html#install-on-mac-os-x-homebrew

注意すべきは `//` はよくあるコメントではなく、パラメータとしてターミナルに入力します。
以下、 `//` 以下も含めて全て入力するのが正しいです。

```
bazel mobile-install //java/com/google/android/apps/authenticator
```


実機インストール後にフリーの OTP QR 生成サイトで動作確認します。

https://freeotp.github.io/qrcode.html

桁数を 6桁から増やしても生成されるパスコードは 6桁のままなので、アプリ側でパラメータを調整可能になっていない可能性があります。

