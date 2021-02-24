---
title: "@RecentlyNullable とは"
emoji: "📘"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# `@RecentlyNullable` とは

- 1. `@RecentlyNonNull` または `@RecentlyNullable` ：スタブソースファイルでのみ生成される注釈。
- 2. コードリンターおよびIDE warning/errors .

> 通常、Kotlinでのnullability契約違反はコンパイルエラーになります。ただし、新しく注釈が付けられたAPIが既存のコードと互換性があることを確認するために、Kotlinコンパイラチームが提供する内部メカニズムを使用して、APIを最近注釈が付けられたものとしてマークします。最近注釈が付けられたAPIは、Kotlinコンパイラーからのエラーではなく、警告のみになります。

つまり、 `@RecentlyNotNull/Nullable` は最近ついた注釈なので対応の猶予期間をコンパイラ側で調整できるようにし、 Kotlin ではエラーではなく、警告として表示しますよ、という理解であっているだろうか

https://stackoverflow.com/questions/53058975/what-is-the-recentlynonnull-annotation

https://www.it-swarm.jp.net/ja/java/recentlynonnull%E3%82%A2%E3%83%8E%E3%83%86%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A8%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B%EF%BC%9F/806767759/
