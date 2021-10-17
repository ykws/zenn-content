---
title: "GoogleAuthenticatorをソースからビルドしてみよう"
emoji: "⛳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [onetimepassword, android, bazel]
published: false
---

二段階認証のパスコードに Google Authenticator を利用している人も多いと思います。

## iOS
https://apps.apple.com/us/app/google-authenticator/id388497605

## Android
https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2

Google Authenticator for Android のオープンソース版があります。
2020年6月9日が最終更新日でアーカイブなっており、現在はメンテナンスされていません。

https://github.com/google/google-authenticator-android

今回はこのオープンソース版を試してみます。

README に書いてある通りです。

# 環境構築
## Android Studio
Android Studio Arctic Fox 2020.3.1 Patch 3
https://developer.android.com/studio

## Bazel
bazel の M1 対応は済んでおり、 Homebrew 経由でインストールすると問題なく動かせます。
https://docs.bazel.build/versions/main/install-os-x.html#install-on-mac-os-x-homebrew

# ソースを取得 
まずは GitHub からソースを取得します。

# パスを通す
ANDROID_HOME として Android SDK のパスを通します。

```
% echo $ANDROID_HOME
/Users/ykws/Library/Android/sdk
```

# ビルド
bazel を利用してビルドします。

注意すべきは `//` はよくあるコメントではなく、パラメータとしてターミナルに入力します。
以下、 `//` 以下も含めて全て入力するのが正しいです。

```
% bazel mobile-install //java/com/google/android/apps/authenticator
```

# 動作確認
実機インストール後にフリーの OTP QR 生成サイトで動作確認します。

https://freeotp.github.io/qrcode.html

桁数を 6桁から増やしても生成されるパスコードは 6桁のままなので、アプリ側でパラメータを調整可能になっていない可能性があります。

