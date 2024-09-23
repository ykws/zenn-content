---
title: "Kotlin の標準ライブラリで UUID が使えるようになりました"
emoji: "🆔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [kotlin]
published: false
---

Kotlin 2.0.20 からついに Kotlin の標準ライブラリとして UUID が扱えるようになりました。

https://kotlinlang.org/docs/whatsnew2020.html#support-for-uuids-in-the-common-kotlin-standard-library

:::message alert
ただし、現時点では **Experimental** となっています。
利用する際は、 `@ExperimentalUuidApi` の Annotation か `-opt-in=kotlin.uuid.ExperimentalUuidApi` のコンパイルオプションを付与する必要があります。
:::

## これまで
Kotlin で UUID を扱うときは Java の UUID を利用していました。

https://docs.oracle.com/javase/jp/17/docs/api/java.base/java/util/UUID.html

今現在 Java の UUID を利用している部分はそのまま利用し続けて問題ありません。

### Kotlin Mutliplatform
Java の UUID で問題になるケースは Kotlin Multiplatform にありました。
Kotlin の実装でないと Kotlin Multiplatform として組み込むことはできなくて、これまで Kotlin Multiplatform で UUID を扱う場合はサードパーティのライブラリを利用していました。
GitHub で現時点で一番スター数が多いのは下記のライブラリです。

https://github.com/benasher44/uuid

## これから
Kotlin 2.0.20 以降であれば、これまでサードパーティに依存していた部分を標準ライブラリに置き換えることが可能になります。

まだ **Experimental** の段階でも積極的に置き換えを進める動機として、他の依存関係が理由になることがあります。

### kotlinx-serialization-json
kotlinx-serialization-json はすでに 1.7.2 から Kotlin Uuid を採用しており、以降のバージョンを利用する際にサードパーティの UUID を併用していると Runtime Exception が発生します。

https://github.com/Kotlin/kotlinx.serialization/releases/tag/v1.7.2

https://github.com/Kotlin/kotlinx.serialization/pull/2744

### Kable
また直接自身のアプリで UUID を利用していなくても間接的に UUID を利用しているケースにおいてもこの組み合わせを考慮する必要があります。

例えば、Kotlin Multiplatform Bluetooth Low Enagy のライブラリ Kable はサードパーティの UUID ライブラリに依存しています。そのため、 kotlinx-serialization-json を先に 1.7.2 に上げることはできなかったりします。

Kable の Kotlin Uuid の対応は 0.36.0 で予定しています。

https://github.com/JuulLabs/kable/pull/758

### Migration
以下は com.benasher44.uuid から kotlin.uuid への migration の一例です。

```diff
- import com.benasher44.uuid.uuidFrom
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = uuidFrom("00002902-0000-1000-8000-00805f9b34fb")
+ val uuid = Uuid.parse("00002902-0000-1000-8000-00805f9b34fb")
```

java.util.UUID -> kotlin.uuid.Uuid

```diff
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.toKotlinUuid

+ @OptIn(ExperimentalUuidApi::class)
private val descriptor: PlatformDescriptor?
-   get() = descriptors.firstOrNull { uuid == it.uuid }
+   get() = descriptors.firstOrNull { uuid == it.uuid.toKotlinUuid() }
```

kotlin.uuid.Uuid -> java.util.UUID

```diff
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.toJavaUuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = ParcelUuid(value.uuid)
+ val uuid = ParcelUuid(value.uuid.toJavaUuid())
```

fromLongs

```diff
- import com.benasher44.uuid.Uuid
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = Uuid(mostSignificantBits, leastSignificantBits)
+ val uuid = Uuid.fromLongs(mostSignificantBits, leastSignificantBits)
```

Generate

```diff
- import com.benasher44.uuid.uuid4
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = uuid4().toString()
+ val uuid = Uuid.random().toString()
```