---
title: "モバイルアプリの操作方法を疑ってみる"
emoji: "🕶️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,android]
published: true
publication_name: yumemi_inc
---

> Think different.[^think-different]

[^think-different]: https://ja.wikipedia.org/wiki/Think_different 1997 年の Apple Computer の広告キャンペーンのスローガン。

## はじめに

筆者は 2004 年からモバイルアプリの開発に携わってきました。20 年経過しようとしています。その間の変遷を経て、現在のモバイルアプリは、Apple[^apple] と Google[^google] それぞれが提供するモバイル OS（iOS[^ios] と Android[^android]）向けに開発しています。モバイルアプリはこれまで、タッチスクリーンを主軸に大きく発展してきました。そして、2023 年 Apple から visionOS[^vision-os] が発表されました。空間コンピューティングという言葉で示されるように、visionOS では iOS での固定されていた**画面単位**という概念を大きく変えることでしょう。この記事では、visionOS が iOS  から変えていく具体的な内容について踏み込めていません。変わっていくであろうこれからに備えるために筆者が必要だと感じていることを共有します。

まず、**インタフェース**[^interface]に関する筆者の考えと関連書籍を紹介します。その後、既成の操作方法を疑ってみて新しいインタフェースを作ってみます。

[^apple]: https://www.apple.com/jp/
[^google]: https://about.google/intl/ALL_jp/
[^ios]: https://www.apple.com/jp/ios/ios-17/
[^android]: https://www.android.com/intl/ja_jp/
[^vision-os]: https://developer.apple.com/jp/visionos/ https://developer.apple.com/documentation/visionOS
[^interface]: インタフェースの表記は、本記事では左記に統一します。書籍の名前や引用はその表記に従うため、表記のぶれがあります。

### 対象者
- モバイルアプリ開発に関わる人
- インタフェースに関心のある人

## インタフェースとは何か

インタフェースとは何でしょうか？

何かと何かをつなぐ時に、**その間を仲介する**ものをインタフェースと呼んでいます。何かと何かなので、モノとモノ、たとえば、電源プラグとコンセントの関係もインタフェースになります。インタフェースは**仲介手続きを定義**します。電池を例にすると、電池のソケットに対応していない電池は受け付けません。許可不許可を明確に区別します。プログラミングにおけるインタフェースもこの概念を表現しています。

モバイルアプリにおけるインタフェースは何でしょうか？

スマートフォンとスマートフォンを使う人の間にあるものはすべてモバイルアプリのインタフェースになります。今は主に**画面**が中心のため、画面に表示されるものを総称して **UI** と呼んだりします。UI は、User Interface の略称で、人（ユーザー）に対して何かをつなぐインタフェースです。UI は画面に限定されるものではないのですが、画面が主流のため、UI といったら画面を指すことがほとんどです。モバイルアプリの画面は、ユーザーに対するインタフェースになっています。ガラケー[^feature-phone]の頃は出力しかできなかった画面が、スマートフォン以降搭載されたタッチスクリーンにより、出力と入力の両方の役割を担い、その大きさは年々大きくなっています。画面以外にも、たとえば、次のようにユーザーに対して機能を提供するインタフェースがあります。

- マイクとスピーカーによる音声の入出力
- 位置情報の送受信
- カメラによる映像の入力（顔認証含む）
- 指紋認証
- 電源ボタン
- マナーモードボタン
- 音量調整ボタン
- 振動で着信を伝える

モバイルアプリの画面のインタフェースの特徴として、さまざまな形と意図をもつ UI  を自由に変更・配置できます。箇条書きで示したハードウェアに依存する固定化された役割ではなく、さまざまな表現と役割を担うことが可能になっています。UI と呼ぶと画面のレイアウトに限定されてしまっている傾向が強くなっている印象がありますが、UI は画面に限定されないです。

visionOS を見た時に今のインタフェースの主役である画面の役割が変わると思いました。もちろん、iOS アプリの延長として visionOS の空間に再配置可能ですが、折角固定化された画面がなくなるのであれば、変えたいという思いと使命感があります。

[^feature-phone]: ガラパゴス・ケータイの略称。スマートフォン登場後、スマートフォン登場前の携帯電話と区別するためにそう呼んでいます。フィーチャーフォンと呼ばれることもあります。ガラケーでは、画面にタッチして入力できなくて、物理的なハードキーが付属しており、ボタンで入力していました。

## これまでのインタフェース

これまでのインタフェースの変遷を知るのに、モバイルアプリは、実にその多くを失ってしまいました。デバイスも OS もアプリも当時を知るのに**紙媒体の方が永く残る**のを目の当たりにしています。ここでは、これまでのインタフェースを知る手掛かりとなる本を紹介します。

2011 年頃の当時のインタフェースを知るには、同年に出版された『iPhoneアプリ設計の極意　思わずタップしたくなるアプリのデザイン』[^tapworthy]という本が当時の制約の中でどのようにデザインしていくかに言及しています。スクリーンショットを見ると、今と比べると小さな画面の中で効果的なインタラクティブを追求する過程も見られます。副題が **『思わずタップしたくなるアプリのデザイン』**とあり、タップ中心の文化形成の起点にもなっています。この副題が集約された部分を引用します。


> この場合の「デザイン」とは単に見た目だけでなく、アプリの機能、性能、ユーザーインターフェースまで含みます。「タップしたくなってしまう」アプリ、別の言い方をすれば「タップする価値のある」アプリは機能でも見た目でもユーザーを引きつけます。

タップしたくなるアプリの入口としてのアイコンについても詳細に触れています。2022 年に出版された『iOSアプリアイコン図鑑』[^ios-app-icons]と合わせて読むと、当時から 10 年以上経過した歴史の変遷も体感でき、アイコンの見方が深まります。

2015 年にはそれまでのモバイルデザインのパターンをまとめた『モバイルデザインパターン ユーザーインタフェースのためのパターン集』[^mobile-design-pattern-gallery]が出版されています。現在も配信中のアプリも事例として掲載されています。アップデートにより今ではもう見ることのできない当時のバージョンのものや、配信停止や対応 OS の関係でインストールや起動できなくなってしまったアプリのスクリーンショットを通してインタフェースのパターンを学ぶことができます。この本の序文からインタフェースの学習方法について引用します。

> 最善の学習方法は視覚的な例に触れることであると考えます。私は物書きであり、言葉が大好きです。望まれるなら、パターンとは何か、どのパターンを選ぶべきか、それぞれのパターンの違いは何かといった点についていくらでも言葉を書き連ねるでしょう。しかし、単にインタフェースをデザインしようとする人（言い換えるなら、1つのツールとしてパターンを学ぼうとおする人）にとって、言葉は明らかに不必要です。パターンを「感じ取る」ことができる程度の説明と、厳選されたさまざまな事例さえあれば、そのパターンを身につけて知識を強固なものにできるはずです。

2018 年『UI GRAPHICS』[^ui-graphics]では、デザイナ視点での知見が詰まっています。副題に**『成功事例と思想から学ぶ、これからのインターフェイスデザインとUX』**とあり、成功事例として当時のアプリのスクリーンショットが見られます。思想にも触れていて、この辺りは副題のとおり、これからのインタフェースを考える上でも有用な概念です。新しいインタフェースを考える上で重要な**自己帰属感**について触れた一節があるので引用します。

> 　インターフェイス設計が、多様なセンサーをベースに、行為とグラフィック（対象）の関係性というインタラクションを設計することとなると、目的への効率性としての使いやすさの観点だけでなく、この自己帰属感をうまく設計しないことには、効率性以前の問題になってしまう。つまり、身体の延長感が得られず、自分の体験にならず、接することに不快感を覚えることになってしまう。
>
> 　逆に自己帰属感がうまく設計できれば、身体の延長、一部になるかのような操作感を提供することができる。そして、インターフェイスで重要とされる、そのデバイスやインターフェイスそのものを意識させない透明性が得られる。結果的にユーザーは、目的のタスクやコンテンツに集中することができる。また、そのデバイスと接すること自体が、ユーザーの新しい体験をスムーズに広げる。自己帰属感の低いインタラクションは、いつまでたっても操作の対象としての意識がつきまとい、身体に近寄ってこない。これが不快感の原因である。

当時を振り返ったり、この辺りの書物に触れると iPhone SDK[^iphone-sdk] の功績にはデザイナとプログラマの境界線を曖昧にしたというのが多いと感じています。それによって、相互に越境し合ってモバイルデザインが急速に発展していった印象を持っています。

[^tapworthy]: JOSH CLARK, Tapworthy DESIGNING GREAT iPHONE APPS, O'REILLY, 2011, [深津貴之 監訳, 武舎広幸・武舎るみ 訳, iPhoneアプリ設計の極意－－思わずタップしたくなるアプリのデザイン, オライリー・ジャパン, 2011]
[^ios-app-icons]: マイケル・フラルップ, iOSアプリアイコン図鑑, ホビージャパン, 2022
[^mobile-design-pattern-gallery]: Theresa Neil, Mobile Design Pattern Gallery Second Edition UI Patterns for Smartphone Apps, O'REILLY, 2015, [深津貴之 監訳, 牧野聡 訳, オライリー・ジャパン, 2015]
[^ui-graphics]: 安藤剛・水野勝仁・萩原俊矢・ドミニク・チェン・菅俊一・鹿野護・有馬トモユキ・渡邊恵太・須齋佑紀/津﨑将氏, 【新版】UI GRAPHICS 成功事例と思想から学ぶ、これからのインターフェイスデザインと UX, BNN, 2018 / 2015 年刊行の同書籍の改訂版として出版されました
[^iphone-sdk]: 当初は iPad もなく、 iPhone のみをターゲットとしていたため、 Apple から提供される Software Development Kit をそう呼んでいました。

## これからのインタフェース

これからのインタフェースを考えるにあたって、2015 年に出版された『さよなら、インタフェース』[^no-ui]という本を紹介します。副題が **『脱「画面」の思考法』** とあり、タイトルでのインタフェースは画面を指していると思ってよいです。この本の中で印象的な部分を引用します。

> 本書の中で、筆者はユーザーインプットではなく、マシンインプットを考えろと言う。ユーザーが何かを入力するのではなく、マシンがさまざまな情報を自動的に取得し、それを元にユーザーの必要なものを提供するのがこれからのインターフェースだと。

『さよなら、インタフェース』の帯には**「時代は、NoUIへ。」**と掲げられています。これを見た時に、次に来るぞと思いました。2015 年でした。当時、Alexa[^alexa] の登場もあって音声操作が主流になる可能性も感じていました。あれから 8 年経過しましたが、NoUI どころかアプリの画面数もユーザー入力も益々増加している印象です。その大きな要因として、モバイルアプリ市場の拡大と大規模開発化してしまった点にあります。モバイルアプリ開発の黎明期は一人で担当することが常でした。2023 年の現在は、チームで開発することが圧倒的に増えました。これからのモバイルアプリの進化のためのひとつの方法として、チームの規模を小さくして、極端には一人でモバイルアプリ開発を進める体制も有効と考えています。その限られたリソースから必然のアイデアとして、NoUI やマシンインプットを考えることが必要不可欠となり、進化を促進させる起爆剤になり得ます。

なお、Apple[^human-interface-guidelines] と Google[^material-design] からもインタフェースのガイドラインは提示されています。画面中心ですが、ユーザーインプットに関するガイドライン[^human-interface-guidelines-input]も用意されています。

[^no-ui]: Golden Krishna, THE BEST INTERFACE IS NO INTERFACE, BNN, 2015, [武舎広幸 監訳, 武舎るみ 訳, さよなら、インタフェース　脱「画面」の思考法, BNN, 2015]
[^alexa]: https://www.amazon.co.jp/meet-alexa/b?ie=UTF8&node=5485773051
[^human-interface-guidelines]: https://developer.apple.com/jp/design/human-interface-guidelines/ https://developer.apple.com/jp/design/tips/ https://developer.apple.com/jp/design/human-interface-guidelines/designing-for-visionos visionOS 向けのデザインのガイドもあります。
[^material-design]: https://m3.material.io/ Material Design is an adaptable system of guidelines, components, and tools that support the best practices of user interface design. と説明があり、ユーザーインタフェース設計のベストプラクティスと銘打たれています。
[^human-interface-guidelines-input]: https://developer.apple.com/jp/design/human-interface-guidelines/inputs

## 新しいインタフェースを作る

ここからは実演として既成のインタフェースを変えてみましょう。

今日において多くの人がディスプレイをタッチスクロールして情報を見ています。そのタッチスクロールのユーザー操作を取り除く UI の案を紹介します。

### ユースケース

たとえば、あなたが電車に乗って、車内がそこそこ混んでいたとしましょう。入り口に留まり続けるのも同調圧力がそうさせません。車内の奥へ進み、右手で吊り革に掴まって立った状態です。徐にスマートフォンを取り出して、X（Twitter）[^x-twitter] のアイコンをタップします。気になるポストがなくて、左手でもつスマートフォンを親指でスクロールして気になるポストを探しています。

片手でタッチスクロールするのは、大変です。実は、昔のスマートフォンは今よりも小さくて、その操作が今に比べると楽でした。徐々に大きく重くなるスマートフォンに人間の方が訓練されてあまり苦ではないという人もいます。しかし、マシンインプットで考えてみましょう。ユーザーが操作に慣れるのではなく、マシンの方に頑張ってもらう方法はないでしょうか？　片手でタッチスクロールせずに**タッチスクロールと同等のマシンインプットを実現する方法**はあるでしょうか？　たとえば、片手でもつスマートフォンを傾けたら、その傾斜に応じてスクロールしてみたらどうでしょうか？

### サンプルアプリ

タッチスクロールをセンサーを利用したスクロールに変えるサンプルアプリを用意しました。

<img alt="ソースコードへの QR コード" style="float:right;margin-left:16px" width="80" src="./images_kawashima/qrcode-github.png">

下記 URL または QR コードからソースコードを入手可能です。お手元の iPhone または Android デバイスにインストールすることで実際に UI の触り心地を確かめてみてください。

- https://github.com/ykws/motion-scroll-app

### コードの解説
#### 傾きの検知を利用できるようにする
##### iOS

Core Motion[^core-motion] を利用して iPhone の傾きを取得できます。次のように `import` すれば使えるようになります。


```swift
import CoreMotion
```

##### Android

SensorManger[^sensor-manager] を利用して Android の傾きを取得できます。Manifest ファイルでの宣言が必要です。

```xml
<uses-feature android:name="android.hardware.sensor.accelerometer" />
```

#### 傾きの検知を開始する
##### iOS

`CMMotionManager`[^motion-manager] の `startAccelerometerUpdates`[^start-accelerometer-updates] を呼ぶことで傾きの検知を開始できます。クロージャに傾きが更新された時の処理を書きます。更新の頻度は `accelerometerUpdateInterval`[^accelerometer-update-interval] で指定できます。


```swift
let motionManager = CMMotionManager()

// 更新の頻度を指定できます
motionManager.accelerometerUpdateInterval = 0.1

motionManager.startAccelerometerUpdates(to: .main) { [weak self] (data, error) in
    // 傾きが更新された時の処理
}
```

##### Android

`SensorManager` の `registerListener`[^register-listener] に `SensorEventListener`[^sensor-event-listener] を登録して、傾き更新の通知を受け取ることができるようになります。

```kotlin
val listener = object : SensorEventListener {
    override fun onAccuracyChanged(sensor: Sensor?, accuracy: Int) {}
    override fun onSensorChanged(event: SensorEvent) {
        // 傾きが更新された時の処理
    }
}

val sensorManager = context.getSystemService(SensorManager::class.java)
val accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
sensorManager.registerListener(listener, accelerometer, SensorManager.SENSOR_DELAY_GAME)
```

#### 傾きの検知を終了する
##### iOS

`CMMotionManager` の `stopAccelerometerUpdates`[^stop-accelerometer-updates] を呼ぶことで傾き検知を終了できます。`deinit` などで呼ぶようにしましょう。

```swift
motionManager.stopAccelerometerUpdates()
```

##### Android

`SensorManager` の `unregisterListener`[^unregister-listener] を呼ぶことで傾きの検知を終了できます。`onDispose` など呼ぶようにしましょう。

```kotlin
sensorManager.unregisterListener(listener)
```

#### ボタンと組み合わせてスクロールを制御する

無制限に傾きでスクロールするのは操作しづらいので、左下にボタンを配置します。ボタンをタップしている間はスクロールするように制御を変えます。ボタンをタッチした時の傾きを基準として、そこから手前に傾けると下に、奥に傾けると上にスクロールさせます。

##### iOS

UIKit[^ui-kit] ベースでの実装サンプルになっています。当初 SwiftUI[^swift-ui] での実装を試していたのですが、実現が難しそうなので採用を見送りました。  `ScrollViewReader`[^scroll-view-reader] を利用すると `ScrollViewProxy` を扱えるようになり `ScrollView` をコードから操作できるようになっています。公開されている `scrollTo`[^scroll-to] は `id` を渡して任意の位置へスクロールする想定になっています。今回は座標ベースの処理となるので採用を見送りました。


```swift
motionManager.startAccelerometerUpdates(to: .main) { [weak self] (data, error) in
    guard let self = self,
          let data = data else { return }

    // ボタンをタッチした時の傾きを基準とする
    // 初期値を nil にしておくことで、開始時の傾きだけ取得して更新し、
    // 終了時に解放されるまでは更新しない
    if (baseAccelerationY == nil) {
        baseAccelerationY = data.acceleration.y
    }

    // 基準の軸から今の傾きの差分を求めて、任意の係数で補正します
    // 補正する数字を大きくするほど、小さな傾きで大きく動くようになります
    let calculatedOffsetY = (baseAccelerationY! - data.acceleration.y) * 300

    // 今時点の UITableView の y 軸のオフセットに加算します
    tableViewOffsetY += calculatedOffsetY

    // スクロール範囲での y 座標の最大値を求めます
    let maxY = tableView.contentSize.height - tableView.bounds.height + view.safeAreaInsets.bottom

    // y 座標が範囲内に収まるように補正します
    if (tableViewOffsetY < 0) { tableViewOffsetY = 0 }
    if (maxY < tableViewOffsetY) { tableViewOffsetY = maxY }

    // 更新された y 座標を UITableView に反映します
    tableView.setContentOffset(CGPoint(x: 0, y: tableViewOffsetY), animated: true)
}
```

##### Android

Jetpack Compose[^jetpack-compose] では、ボタンをタップしている間を `pointerInput`[^pointer-input] の `detectTapGesture`[^detect-tap-gestures] で実現できます。`onPress` スコープの中で `tryAwaitRelease`[^try-await-release] するとタップ解除されるまで待つことができます。Jetpack Compose では他にもさまざまなクリック処理[^compose-click-handler]が可能です。

```kotlin
Box(modifier = Modifier.pointerInput(Unit) {
    detectTapGestures(
        onPress = {
            // タップされた時の処理

            // タップ解除されるまで待つ
            tryAwaitRelease()

            // タップ解除された時の処理
        }
    )
}
```

傾きに合わせてスクロールの値を算出します。

```kotlin
val density = LocalDensity.current.density

override fun onSensorChanged(event: SensorEvent) {
    // TYPE_ACCELEROMETER を指定すると、
    // SensorEvent.values[1] から、 y 軸方向の加速力（重量を含む）を取得できます
    //
    // https://developer.android.com/guide/topics/sensors/sensors_motion

    // ボタンをタッチした時の傾きを基準とする
    if (baseY == null) baseY = event.values[1]
    
    // 基準の軸から今の傾きの差分を求めて、任意の係数で補正します
    scrollOffset = (event.values[1] - (baseY ?: 0f)) * 100 * density
}
```

上記で算出した値に応じて UI に反映します。

```kotlin
// 傾きから算出した scrollOffset が変化したらスクロール処理する
LaunchedEffect(scrollOffset) {
    scrollState.animateScrollBy(
        value = scrollOffset,
        animationSpec = tween(durationMillis = 200, easing = LinearEasing),
    )
}
```

[^x-twitter]: https://about.twitter.com/ja 執筆時点で、ブランド名は X に変わりましたが、ドメイン含めて Twitter の名前が残っている状態です。
[^core-motion]: https://developer.apple.com/documentation/coremotion
[^sensor-manager]: https://developer.android.com/guide/topics/sensors/sensors_overview
[^motion-manager]: https://developer.apple.com/documentation/coremotion/cmmotionmanager
[^start-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616148-startaccelerometerupdates
[^accelerometer-update-interval]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616135-accelerometerupdateinterval
[^register-listener]: https://developer.android.com/reference/android/hardware/SensorManager#registerListener(android.hardware.SensorEventListener,%20android.hardware.Sensor,%20int)
[^sensor-event-listener]: https://developer.android.com/reference/android/hardware/SensorEventListener
[^stop-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616138-stopaccelerometerupdates
[^unregister-listener]: https://developer.android.com/reference/android/hardware/SensorManager#unregisterListener(android.hardware.SensorEventListener)
[^ui-kit]: https://developer.apple.com/jp/documentation/uikit/
[^swift-ui]: https://developer.apple.com/jp/xcode/swiftui/
[^scroll-view-reader]: https://developer.apple.com/documentation/swiftui/scrollviewreader
[^scroll-to]: https://developer.apple.com/documentation/swiftui/scrollviewproxy/scrollto(_:anchor:)
[^jetpack-compose]: https://developer.android.com/jetpack/compose?hl=ja
[^pointer-input]: https://developer.android.com/jetpack/compose/touch-input/pointer-input
[^detect-tap-gestures]: https://developer.android.com/reference/kotlin/androidx/compose/foundation/gestures/package-summary#(androidx.compose.ui.input.pointer.PointerInputScope).detectTapGestures(kotlin.Function1,kotlin.Function1,kotlin.coroutines.SuspendFunction2,kotlin.Function1)
[^try-await-release]: https://developer.android.com/reference/kotlin/androidx/compose/foundation/gestures/PressGestureScope
[^compose-click-handler]:  https://star-zero.medium.com/compose%E3%81%AE%E6%A7%98%E3%80%85%E3%81%AA%E3%82%AF%E3%83%AA%E3%83%83%E3%82%AF%E5%87%A6%E7%90%86-dd66f19992c5 Jetpack Compose のさまざまなクリック処理について紹介されています。

<hr class="page-break" />

## おわりに

今回紹介した案は、当たり前になっているけどユーザー操作量が多く、それをなくす、もしくは減らすことはできないかというユースケースに取り組みました。この方法がベストだとかそういうことを言いたいのではなく、当たり前になっている UI も変えることが可能であり、それをプログラマは**すぐに試すことができる**ということを伝えたかったです。

スマートフォンには他にもセンサーが搭載されていて、Apple と Google はさまざまな機能を開放しています。

現代では、チーム開発が主流となっています。UI/UX はデザイナが考えます。スマートフォンの特性を活かして、**マシンにユーザーを従わせるのではなく、ユーザーにマシンが従うようにシステムを設計・構築するのはプログラマの役目**だと考えています。大規模なチーム開発では、インタフェースを設計・構築する役割分担があり、何かを試行錯誤する機会を得るのは難しいです。

そこでぜひ、小規模で、**可能なら一人で**モバイルアプリのインタフェースの設計と構築をしてみてほしいです。そうすることで NoUI やマシンインプットの観点で再設計せざるを得ない機会が増えると考えています。そして来るべき visionOS の時代に備えたインタフェースを一緒に模索していきたいです。そのための基礎体力として、既成のモバイルアプリのインタフェースにおいても十分に改良の余地があり、拡張または再構築していく体験が重要になります。もしモバイルアプリを触っていて、操作しづらいなあ、面倒だなと感じることがあれば、そこには新しいインタフェースが必要です。一緒に新しいインタフェースを作りましょう。

### ヒント

- スマートフォンに搭載されているセンサーにはどんなものがあるか調べてみよう
- SDK が提供している API にはどんなものがあるか調べてみよう
- 面倒だったのに慣れてしまった操作としてどんなものがあるか思い出してみよう
- もし画面がなかったら（あるいは目が見えない人に）どんな表現で情報を伝えることができるか考えてみよう
