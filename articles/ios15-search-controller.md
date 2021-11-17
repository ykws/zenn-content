---
title: "iOS15 で UISearchController を利用する時の注意点"
emoji: "💡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios, uisearchcontroller]
published: false
---

# UISearchController でコンテンツを隠すとは

iOSアプリで [UISearchController](https://developer.apple.com/documentation/uikit/uisearchcontroller) を利用している場合、検索中に基になるコンテンツを隠すかどうか変更できます。

このように検索中に下の TableView の Cell をタップできない状態を基になるコンテンツを隠すと表現しています。グレーでマスクされている部分をタップすると Cancel をタップした時と同じように検索中を解除します。

![](https://storage.googleapis.com/zenn-user-upload/3411bb30a106-20211117.png =200x)

隠したい場合 iOS14まではデフォルトで隠す設定だったので、特に意識しないで済みました。
これが iOS15以降ではデフォルトで隠さない設定に変わってしまったため、隠したい場合は明示的に隠す設定にする必要があります。

iOS15以降では検索中グレーでマスクされなくなります。そのため、検索中に下の TableView の Cell をタップできる状態になっています。

![](https://storage.googleapis.com/zenn-user-upload/be948a029c0b-20211117.png =200x)

## コンテンツを隠すかどうかの property

### iOS13以降
[obscuresBackgroundDuringPresentation](https://developer.apple.com/documentation/uikit/uisearchcontroller/1618656-obscuresbackgroundduringpresenta)

### iOS13未満

:::message
iOS13 以降では Deprecated です。
:::

[dimsBackgroundDuringPresentation](https://developer.apple.com/documentation/uikit/uisearchcontroller/1618660-dimsbackgroundduringpresentation)

---

- iOS15未満ではデフォルト `true`
- iOS15以降はデフォルト `false`

よって iOS15 以降で検索中に基になるコンテンツを**隠したい**場合は

```swift
searchController.obscuresBackgroundDuringPresentation = true
```

逆に iOS14未満で検索中に基になるコンテンツを**隠したくない**場合は

```swift
searchController.obscuresBackgroundDuringPresentation = false
```

さらに iOS13 未満もサポートする場合は

```swift
if #available(iOS 13.0, *) {
  searchController.obscuresBackgroundDuringPresentation = false
} else {
  searchController.dimsBackgroundDuringPresentation = false
}
```

iOS15以降で隠さない場合、iOS15未満で隠したい場合は明示する必要はないです。

### 同時に設定すると？

Warning が表示されますが、 Deprecated なので、まだどちらの property も使用可能です。
これを同時に利用するとどうなるかというと、現時点では、 `dimsBackgroundDuringPresentation` の設定が優先されます。

以下のコードでは、コンテンツは隠されないです。

```swift
searchController.obscuresBackgroundDuringPresentation = true
searchController.dimsBackgroundDuringPresentation = false
```

以下のコードでは、コンテンツは隠されます。

```swift
searchController.obscuresBackgroundDuringPresentation = false
searchController.dimsBackgroundDuringPresentation = true
```

いずれも推奨される書き方ではないですが、既存のコードで意図しない挙動をしていたら注目すべき点です。

## OS標準では
なお、OS標準のアドレス帳ではこのデフォルト値に従っていました。
つまり、

### iOS14

iOS14までは検索中にアドレス帳のコンテンツをタップできませんでしたが、

![](https://storage.googleapis.com/zenn-user-upload/76fdd50430bd-20211117.png =200x)

### iOS15

iOS15以降、検索中にアドレス帳のコンテンツをタップできるようになっています。

![](https://storage.googleapis.com/zenn-user-upload/f05ec901c14d-20211117.png =200x)

ゆえに Apple の期待する UISearchController の利用方法としては、デフォルトの挙動に従うべきなのかもしれません。
[Human Interface Guidelines - Search Bars](https://developer.apple.com/design/human-interface-guidelines/ios/bars/search-bars/) からはどのような振る舞いを期待しているかは読み取れませんでしたので、リジェクト対象になるとは考えにくいです。
ただ、標準の動作に適合していく方が今後の OS のアップデートに追従していくのが容易になる可能性も感じました。

## 検証リポジトリ
これを検証したソースコードのリポジトリです。

[SearchExample](https://github.com/ykws/SearchExample)

