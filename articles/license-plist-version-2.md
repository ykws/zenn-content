---
title: "Xcode13.3からのLicensePlist利用の注意点"
emoji: "🔧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,license,xcode,spm]
published: false
---

iOSアプリに利用しているライブラリのライセンス表記を実装するのに [LicensePlist](https://github.com/mono0926/LicensePlist) というライブラリが便利です。

しかし、Xcode 13.3 から SPM 連携のライブラリのライセンス表記に利用する際には注意する必要があります。

# それ以前との違い

https://github.com/mono0926/LicensePlist/issues/179

上記 Issue を例にすると、次の表のようにライセンス表記が変わります。

| それ以前 | Xcode 13.3 以降 |
| -- | -- |
| AppCenter | appcenter-sdk-apple |

## 原因

> With the new Package.resolved JSON version 2 (#174), which Apple introduce with Xcode 13.3 and Swift 5.6

Issue の冒頭の記載の通り、 Xcode 13.3 から SPM 連携の結果が出力され、 LicensePlist がライセンス表記に参照する Package.resolved のバージョンが変わってしまいました。 Issue に転記されているバージョンごとのフォーマットの差異を見比べると、元の `"package": "AppCenter"` という情報が失われており、 `"identity": "appcenter-sdk-apple"` の情報が追加されていて、前のバージョンと同じように表示することは不可能に見えます。

## 対策

[Rename](https://github.com/mono0926/LicensePlist#rename) 機能を利用して、ライセンス表記の表示名を設定します。

```license-plist.yml
rename:
  appcenter-sdk-apple: AppCenter
```

元となるソースには情報が存在しないので、対応表を用意して置換していくしかないという判断です。それ以前の表記がわかっていれば、機械的にこの対応は可能ですが、 Xcode 13.3 から利用した場合、それ以前の表記は確認できないので、何を適切な表示とするか決める必要があります。

## 改善案

各リポジトリの Package.swift の `Package(name:` に元の表記を確認でき、おそらく以前の Package.resolved もこの内容を出力していた推測します。 Package.resolved からは location によってリポジトリの URL がわかるので、そこから各リポジトリの Package.swift を走査すれば、元の表記を取得できそうです。

https://github.com/microsoft/appcenter-sdk-apple/blob/a52cb5001c94e86a04bee0ce8d9fc1a73ccf2985/Package.swift#L54

