---
title: "iOS15 SearchController"
emoji: "💡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios, searchcontroller]
published: false
---

iOSアプリで SearchController を利用している場合、
検索中に基になるコンテンツを隠すかどうか変更できます。
隠したい場合 iOS14まではデフォルトで隠す設定のため特に意識しないで済みました。

iOS15以降ではデフォルトで隠さない設定に変わってしまったため、
隠したい場合は明示的に隠す設定にする必要があります。

iOS13以降
obscuresBackgroundDuringPresentation
https://developer.apple.com/documentation/uikit/uisearchcontroller/1618656-obscuresbackgroundduringpresenta

Deprecated, iOS13未満
dimsBackgroundDuringPresentation
https://developer.apple.com/documentation/uikit/uisearchcontroller/1618660-dimsbackgroundduringpresentation

iOS15未満ではデフォルト true
iOS15以降はデフォルト false

よって iOS15 以降で検索中に基になるコンテンツを隠したい場合は

```
searchController.obscuresBackgroundDuringPresentation = true
```

逆に iOS14未満で検索中に基になるコンテンツを隠したくない場合は

```
searchController.obscuresBackgroundDuringPresentation = false
```

さらに iOS13 未満もサポートする場合は

```
if #available(iOS 13.0, *) {
  searchController.obscuresBackgroundDuringPresentation = false
} else {
  searchController.dimsBackgroundDuringPresentation = false
}
```

iOS15以降で隠さない場合、iOS15未満で隠したい場合は明示する必要はないです。

SearchExample
https://github.com/ykws/SearchExample

