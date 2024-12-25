---
title: "iOS Widget と Auth0 の連携"
emoji: "🔐"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,swift,auth0]
published: true
---

iOS アプリで Auth0 の認証機構を利用して Widget と Keychain による認証情報の共有の仕方を解説します。

Auth0 とは認証基盤の IDaaS です。無料枠があり、以降の簡単なサンプルを試す範囲であれば、無料で試せると思います。
https://auth0.com/jp

iOS アプリに Auth0 を組み込む部分は iOS QuickStart が用意されていてわかりやすいです。

https://auth0.com/docs/quickstart/native/ios-swift/interactive

:::details サンプルアプリ確認手順
以下は、サンプルアプリで Auth0 の認証を確認した備忘録です。

1. Quick Start iOS を選択して Sample をダウンロードする
2. Signing 自分の ADP に合わせて Team ID と BundleID を変える
3. Capability から Associated Domains を設定する必要があるがダウンロード版は最初から設定済み
4. Auth0 の Settings で Allowed Callback URLs, Allowed Logout URLs をコピペして BUNDLE_ID を変更して登録する
5. Advanced Settigns で Team ID と App ID を設定する
6. アプリをビルドしてインストール
7. アプリを起動して Login
8. Google Signin
9. ログインできることを確認
10. ログアウトできることを確認
:::

Auth0 には認証情報の管理するヘルパークラスとして **CredentialsManager** を用意しています。

https://auth0.github.io/Auth0.swift/documentation/auth0/credentialsmanager/

CredentialsManager は Keychain を扱い、 Keychain を簡易に扱えるように **SimpleKeychain** も用意しています。

https://auth0.github.io/Auth0.swift/documentation/auth0/simplekeychain

Refresh Token を有効にするには、 scope に `offline_access` を含めてリクエストする必要があります。サンプルと同じように動かすには `profile email` も必要。

アプリと Widget で Keychain を共有するためにはグループの設定が必要です。

https://developer.apple.com/documentation/security/sharing-access-to-keychain-items-among-a-collection-of-apps

Keychain を共有するには、 SimpleKeychain に対して以下を指定する必要があります。

- service
- accessGroup

https://github.com/auth0/SimpleKeychain/blob/master/SimpleKeychain/SimpleKeychain.swift

service は default で `Bundle.main.bundleIdentifier` が指定されており、アプリと Widget では Bundle ID が異なります。そのため、明示的にユニークな ID を指定する必要がある。この service は Keychain を共有するために必要で、共通の値を設定すれば良いので Bundle ID でなくても構わないです。

accessGroup は Keychain Group の値を設定します。 Team ID の prefix も必要です。

この設定によって、アプリでログインして Keychain に保存したログイン情報を Widget からも参照することができるようになります。
