---
title: "Kotlin の標準ライブラリで UUID が使えるようになりました"
emoji: "🔑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [kotlin]
published: false
---

Kotlin 2.0.20 からついに Kotlin の標準ライブラリとして UUID が使えるようになりました。

https://kotlinlang.org/docs/whatsnew2020.html#support-for-uuids-in-the-common-kotlin-standard-library

:::message alert
ただし、現時点では **Experimental** となっています。
利用する際は、 `@ExperimentalUuidApi` の Annotation か `-opt-in=kotlin.uuid.ExperimentalUuidApi` のコンパイルオプションを付与する必要があります。
:::

## これまでの課題
Kotlin で UUID を扱う場合、従来は Java の UUID クラスを使用していました。これは JVM プラットフォームで動作する場合には問題ありませんが、 Kotlin Multiplatform では Java のライブラリを直接利用することができないため、別の方法が必要でした。

https://docs.oracle.com/javase/jp/17/docs/api/java.base/java/util/UUID.html

### Kotlin Mutliplatform
Kotlin Multiplatform では Java の UUID を使用できなかったため、サードパーティのライブラリに依存していました。
GitHub で現時点で一番スター数が多いのは次のライブラリです。

https://github.com/benasher44/uuid

## 移行への注意点
Kotlin 2.0.20 以降であれば、これまでサードパーティに依存していた部分を標準ライブラリに置き換えることが可能になります。
今現在使用中の Java の UUID はそのまま引き続き使用できるので、 Kotlin Multiplatform でなければ無理に移行する必要はありません。

まだ **Experimental** 段階ですが、依存関係が理由で、 UUID の標準ライブラリへの移行が必要になる場合もあります。
一方で、依存関係によって移行が難しい場合もあります。特にまだ Kotlin Uuid に対応していないライブラリに依存している場合、サードパーティの UUID を引き続き使用する必要があるため、移行に制約が生じることがあります。

以下は、ライブラリと UUID の依存関係における事例の紹介です。

### kotlinx-serialization-json
kotlinx-serialization-json はすでに 1.7.2 から Kotlin Uuid を採用しており、以降のバージョンを利用する際にサードパーティの UUID を併用していると Runtime Exception が発生するケースがあります。

https://github.com/Kotlin/kotlinx.serialization/releases/tag/v1.7.2

https://github.com/Kotlin/kotlinx.serialization/pull/2744

例えば、次のようにサードパーティを利用した UUID を Serialize するクラスを定義しているとします。

```kotlin
import com.benasher44.uuid.Uuid
import kotlinx.serialization.Serializable

@Serializable data class User(val uuid: Uuid)
```

この User オブジェクトを ecode しようとすると iOS/Android 上で実行時に Exception が throw されます。

```kotlin
import kotlinx.serialization.encodeToString
import kotlinx.serialization.json.Json

val jsonString = Json.encodeToString(user)
```

#### iOS

```
kotlin.native.internal.IrLinkageError: Reference to class 'Uuid' can not be evaluated: No class found for symbol 'kotlin.uuid/Uuid|null[0]'
```

#### Android

```
kotlinx.serialization.SerializationException: Serializer for class 'User' is not found.
```

このエラーは、 kotlinx-serialization-json 1.7.2 以降が標準ライブラリの kotlin.uuid に依存しているにもかかわらず、サードパーティの benasher44/uuid を使用しているため、 kotlinx-serialization-json 側で koltin.uuid/Uuid のクラス解決ができないことが原因です。
このエラーを回避するには、引き続き kotlinx-serialization-json 1.7.1 を使用するか、サードパーティの UUID を標準ライブラリに切り替える必要があります。

### Kable
また直接アプリケーションで UUID を利用していなくても、間接的に UUID を利用しているケースにおいて、この組み合わせを考慮する必要が生じます。

例えば、Kotlin Multiplatform Bluetooth Low Energy のライブラリ Kable はサードパーティの UUID ライブラリに依存しています。そのため、 kotlinx-serialization-json を 1.7.2 に更新する際に UUID の依存関係が競合し、同様の問題が発生する可能性があります。

Kable の Kotlin Uuid の対応は 0.36.0 のマイルストーンで計画しています。

https://github.com/JuulLabs/kable/pull/758

## Migration
以下は com.benasher44.uuid から kotlin.uuid への migration の一例です。（前述の Kabel の PR からの抜粋です。コンテキストを深く知りたい場合は、PR を参照ください）

### parse
文字列から Uuid を生成します。

```diff
- import com.benasher44.uuid.uuidFrom
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = uuidFrom("00002902-0000-1000-8000-00805f9b34fb")
+ val uuid = Uuid.parse("00002902-0000-1000-8000-00805f9b34fb")
```

### toKotlinUuid
java.util.UUID から kotlin.uuid.Uuid に変換します。

```diff
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.toKotlinUuid

+ @OptIn(ExperimentalUuidApi::class)
private val descriptor: PlatformDescriptor?
-   get() = descriptors.firstOrNull { uuid == it.uuid }
+   get() = descriptors.firstOrNull { uuid == it.uuid.toKotlinUuid() }
```

### toJavaUuid
kotlin.uuid.Uuid から java.util.UUID に変換します。

```diff
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.toJavaUuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = ParcelUuid(value.uuid)
+ val uuid = ParcelUuid(value.uuid.toJavaUuid())
```

### fromLongs
二つの Long から Uuid を生成します。

```diff
- import com.benasher44.uuid.Uuid
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = Uuid(mostSignificantBits, leastSignificantBits)
+ val uuid = Uuid.fromLongs(mostSignificantBits, leastSignificantBits)
```

### random
Uuid を生成します。

```diff
- import com.benasher44.uuid.uuid4
+ import kotlin.uuid.ExperimentalUuidApi
+ import kotlin.uuid.Uuid

+ @OptIn(ExperimentalUuidApi::class)
- val uuid = uuid4().toString()
+ val uuid = Uuid.random().toString()
```

UUID v4 の生成ということで単純に置き換えています。

> The returned uuid conforms to the IETF variant (variant 2) and version 4

https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.uuid/-uuid/random.html

## まとめ
UUID は広く普及しているため、アプリケーションで直接的に、または間接的に利用されることが多くあります。そのため、標準ライブラリへの切り替えは、他の依存関係との兼ね合いも考慮して計画する必要があります。
UUID の切り替えが難しい場合もありますが、依存するライブラリが更新された場合、予期せず切り替えが必要になることも考えられます。UUID の機能がまだ **Experimental** であるため、導入のタイミングは慎重に判断する必要があり、依存ライブラリの動向にも注視しておくことが重要です。
