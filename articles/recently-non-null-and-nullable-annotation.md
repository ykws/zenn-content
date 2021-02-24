---
title: "@RecentlyNullable とは"
emoji: "📘"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# `@RecentlyNonNull/Nullable` とは

1. スタブソースファイルでのみ生成される
2. コードリンターおよびIDEでの warning/errors を制御する

> 通常、Kotlinでのnullability契約違反はコンパイルエラーになります。ただし、新しく注釈が付けられたAPIが既存のコードと互換性があることを確認するために、Kotlinコンパイラチームが提供する内部メカニズムを使用して、APIを最近注釈が付けられたものとしてマークします。最近注釈が付けられたAPIは、Kotlinコンパイラーからのエラーではなく、警告のみになります。

つまり、 `@RecentlyNonNull/Nullable` は最近ついた注釈なので対応の猶予期間をコンパイラ側で調整できるようにし、 Kotlin ではエラーではなく、警告として表示しますよ、という理解であっているだろうか

## 参照した stackoverflow

https://stackoverflow.com/questions/53058975/what-is-the-recentlynonnull-annotation

記事のリンク先が間違っていてこちらのはず

https://android-developers.googleblog.com/2020/03/handling-nullability-in-android-11-and.html

> The Kotlin compiler also recognizes two similar annotations, @RecentlyNullable and @RecentlyNonNull, which are the exact same as @Nullable and @NonNull, only they generate warnings instead of errors[^1].
>
> ### Nullability in Android 11
>
> Last month, we released the Android 11 Developer Preview, which allows you to test out the new Android 11 SDK. We upgraded a number of annotations in the SDK from @RecentlyNullable and @RecentlyNonNull to @Nullable and @NonNull (warnings to errors) and continued to annotate the SDK with more @RecentlyNullable and @RecentlyNonNull annotations on methods that didn’t have nullability information before.
>
> ### What’s next
>
> If you are writing in Kotlin, when upgrading from the Android 10 to the Android 11 SDK, you may notice that there are some new compiler warnings and that previous warnings may have been upgraded to errors. This is intended and a feature of the Kotlin compiler — these warnings tell you that you may be writing code that crashes your app at runtime (a risk you would miss entirely if you weren’t writing in Kotlin). As you encounter these warnings and errors, you can handle them by adding null checks to your code.
>
> As we continue to make headway annotating the Android SDK, we’ll follow this same pattern — @RecentlyNullable and @RecentlyNonNull for one numbered release (e.g., Android 10), and then upgrade to @Nullable and @NonNull in the next release (e.g., Android 11). This practice will give you at least a full release cycle to update your Kotlin code and ensure you’re writing high-quality, robust code.
> 
> [^1]: Due to rules regarding handling of annotations in Kotlin, there is currently a small set of cases where the compiler will throw an error for @Nullable references but not for @RecentlyNullable references.

### 日本語訳

https://www.it-swarm.jp.net/ja/java/recentlynonnull%E3%82%A2%E3%83%8E%E3%83%86%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A8%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B%EF%BC%9F/806767759/
