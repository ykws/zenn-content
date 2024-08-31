---
title: "NumberFormatException"
emoji: "💣"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

Flutter から Kotlin に移植した時にクラッシュしたお話

次のようなコードを Flutter から Kotlin に移植しました。
string には任意の文字列が 16進数表記で指定されます。

```dart
int.parse(string, radix: 16);
```

```kotlin
string.toInt(radix = 16)
```

Kotlin に移植すると特定の条件でクラッシュが発生しました。

原因を辿ると、クラッシュする時、 string には "80000000" が指定されていることがわかりました。

NumberFormatException が throw されていました。

```kotlin
Exception in thread "main" java.lang.NumberFormatException: For input string: "80000000" under radix 16
 at java.lang.NumberFormatException.forInputString (:-1) 
 at java.lang.Integer.parseInt (:-1) 
 at FileKt.main (File.kt:7) 
```

理由は `Int.MAX_VALUE` が 2147483647 で 16進数表記で 7FFFFFFF(16) なので、 8000000(16) は `Int.MAX_VALUE` を超えてしまうからでした。

なぜ Flutter では問題なかったのか、ちなみに iOS の方も問題は発生していませんでした。

Swift のコード

```swift
Int(string, radix: 16)
```

Flutter も iOS も 2147483648 に変換されます。
どちらも 64bit なので Int の最大値は 9,223,372,036,854,775,807 になります。
16進数表記で 7FFFFFFFFFFFFFFF(16) なので、 8000000000000000(16) で最大値を超えます。

Kotlin の toInt の内部では Java の Integer.parseInt が呼ばれて NumberFormatException を throw しています。

これを回避するには、

Kotlin の toInt の内部では Java の Integer.parseInt が呼ばれて NumberFormatException を throw しています。

これを回避するには、

1. toIntOrNull
2. toLong

```kotlin
string.toIntOrNull(radix = 16)?.let { /* process */ }
```

こうしておくと、 null なら何もしないという形で安全に扱えます

toLong にすると、 Long.MAX_VALUE 9,223,372,036,854,775,807 まで扱えるようになります。

Kotlin と Swift で同じ挙動とするコード


```kotlin
string.toLongOrNull(radix = 16)?.let { /* process */ }
```

```swift
if let it = Int(string, radix: 16) { /* process */ }
```

Int の最大値を超えた時に Dart では 9223372036854776000 という中途半端な数値を返します。 DartPad で試したので、この制限かもしれませんん。

> If the source string does not contain a valid integer literal, optionally prefixed by a sign, a FormatException is thrown.

`FormatException` が `throw` されそうですが、確認できていないです。



