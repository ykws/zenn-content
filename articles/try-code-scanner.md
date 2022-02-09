---
title: "Android で QR コードスキャンを実装する"
emoji: "📷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [android,camera,qr]
published: true 
---

# はじめに

Android で QR コードスキャンを実装するのに [code-scanner](https://github.com/yuriy-budiyev/code-scanner) を利用します。README の通りの手順ではうまくいかないので Workaround の記録です。

# Workaround
## 変更後の URL が反映されていない？

README には以下のように記載されていますが、まだ反映されていないのか、 `Failed to resolve: com.github.yuriy-budiyev:code-scanner:2.1.0
` となって失敗します。

```build.gradle
dependencies {
    implementation 'com.github.yuriy-budiyev:code-scanner:2.1.0'
}
```

変更前の URL を指定すると取得可能です。

```diff
dependencies {
-   implementation 'com.github.yuriy-budiyev:code-scanner:2.1.0'
+   implementation 'com.budiyev.android:code-scanner:2.1.0'
}
```

この変更は以下の Pull Request で直近発生していたのでまだ反映されていなくて、直に反映される解消される可能性があります。

https://github.com/yuriy-budiyev/code-scanner/pull/140

## パーミッションの許可を求める

別のデモプロジェクトにあるように API 23 以上ではカメラパーミッションの許可を求める必要があります。

https://github.com/yuriy-budiyev/lib-demo-app/blob/2e5faf841d04eb4c9580ae4bbd9b6dc405c6dddf/app/src/main/java/com/budiyev/android/libdemoapp/codescanner/CodeScannerActivity.java#L55-L64

# 確認リポジトリ

最終的に動作を確認できた状態を以下のリポジトリに反映しました。

https://github.com/ykws/code-scanner-example

