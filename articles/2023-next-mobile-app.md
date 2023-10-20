---
title: "これからのモバイルアプリのカタチ"
emoji: "🕶️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [ios]
published: false
---

## はじめに

筆者は2004年からモバイルアプリケーションプログラマとして携帯電話向けのアプリケーション開発に携わってきました。そこから20年経過しようとしています。モバイルアプリは、これまでタッチスクリーンを主軸に大きく発展してきました。この先モバイルアプリはどのようなカタチになっていくのでしょうか？　この先生きのこるモバイルアプリはどんなカタチをしているでしょうか？　この記事では、これからのモバイルアプリに対する考え方を共有します。

2023年 Apple[^apple] から visionOS[^vision-os] が発表されました。空間コンピューティングという言葉で示されるように、 visionOS では iOS での固定されていた**画面単位**という概念を大きく変えることでしょう。この記事では、 visionOS が iOS から変えていく具体的な内容について踏み込めていません。これから変わっていくであろう**インタフェース**[^interface]に対する**基盤となる考え方**について述べていきます。

[^apple]: https://www.apple.com/jp/
[^vision-os]: https://developer.apple.com/jp/visionos/ https://developer.apple.com/documentation/visionOS
[^interface]: インタフェースの表記は、本記事では左記に統一します。書籍の名前や引用はその表記に従うため、その点において記事中の表記のぶれが発生することがあります。

### 対象者
- モバイルアプリケーションプログラマ
- モバイルアプリケーションデザイナ
- インタフェースに関心のあるすべての人

## インタフェースとは

インタフェースとは何でしょうか？

何かと何かをつなぐ時に、**その間を仲介する**ものをインタフェースと呼んでいます。何かと何かなので、モノとモノ、例えば、電源プラグとコンセントの関係もインタフェースになります。 

モバイルアプリにおけるインタフェースは何でしょうか？

スマホ[^smart-phone]を使う人とスマホの間にあるものは全てインタフェースになります。今は主に**画面**です。画面に表示されるものを総称して **UI** と呼んでいます。 UI は、 User Interface の略称で、人に対して何かをつなぐシーンに限定されます。モバイルアプリの画面は、ユーザに対するインタフェースになっています。ガラケー[^feature-phone]の頃は出力しかできなかった画面が、スマホ以降搭載されたタッチスクリーンにより、出力と入力の両方の役割を担い、その大きさは年々大きくなっています。画面以外にも、例えば、下記のようにユーザに対して機能を提供するインタフェースがあります。

- マイクとスピーカーによる音声の入出力
- 位置情報の送受信
- カメラによる映像の入力（顔認証含む）
- 指紋認証
- 電源ボタン
- マナーモードボタン
- 音量調整ボタン
- 振動で着信を伝える

モバイルアプリの画面のインタフェースの特徴として、様々な形と意図を持つ UI  を自由に変更・配置できるため、箇条書きで示したハードに依存する固定化された役割ではなく、様々な表現と役割を担うことが可能になっています。

visionOS を見た時に今のインタフェースの主役である画面の役割が変わると思いました。もちろん、 iOS アプリの延長として visionOS の空間に再配置することは可能ですが、折角固定化された画面がなくなるのであれば、変えたいという思いと使命感があります。

これからのモバイルアプリのインタフェースを考えるにあたって、人を介さずにやり取りできる観点を意識して持っておきたいので、 UI に限定されない場面では、インタフェースと表記します。

[^smart-phone]: スマートフォンの略称として、本記事では統一してそう呼ぶことにします。
[^feature-phone]: ガラパゴス・ケータイの略。スマートフォン登場後にそれと区別するために、スマートフォン登場前の携帯電話をそう呼んでいます。フィーチャーフォンと呼ばれることもあります。ガラケーでは、画面にタッチして入力することができなくて、物理的なハードキーが付属しており、ボタンで入力を行っていました。

### これからのインタフェース

これからのインタフェースを考えるにあたって、2015年に出版された『さよなら、インタフェース』[^no-ui]という本を紹介します。副題が**『脱「画面」の思考法』**とあり、タイトルでのインタフェースは画面を指していると思ってよいです。この本の中で印象的な部分を引用します。

> 本書の中で、筆者はユーザーインプットではなく、マシンインプットを考えろと言う。ユーザーが何かを入力するのではなく、マシンがさまざまな情報を自動的に取得し、それを元にユーザーの必要なものを提供するのがこれからのインターフェースだと。

『さよなら、インタフェース』の帯には**「時代は、NoUIへ。」**と掲げられています。これを見た時に、次に来るぞと思いました。2015年でした。当時、 Alexa[^alexa] の登場もあって音声操作が主流になる可能性も感じていました。あれから8年経過しましたが、 NoUI　どころかアプリの画面数もユーザ入力も益々増加している印象です。その大きな要因として、モバイルアプリ市場の拡大と大規模開発化してしまった点にあると思います。モバイルアプリ開発の黎明期は一人で担当することが常でした。2023年の現在は、チームで開発することが圧倒的に増えました。これからのモバイルアプリの進化のための一つの方法として、チームの規模を小さくして、極端には一人でモバイルアプリ開発を進める体制も有効であると考えています。その限られたリソースから必然のアイディアとして、 NoUI やマシンインプットを考えることが必要不可欠になると思うのが進化を促進させる起爆剤になる得ると思っています。

[^no-ui]: Golden Krishna, THE BEST INTERFACE IS NO INTERFACE, BNN, 2015, [武舎広幸 監訳, 武舎るみ 訳, さよなら、インタフェース　脱「画面」の思考法, BNN, 2015]
[^alexa]: https://www.amazon.co.jp/meet-alexa/b?ie=UTF8&node=5485773051

### これまでのインタフェース

これまでのインタフェースの変遷を知るのに、モバイルアプリは、実にその多くを失ってしまいました。デバイスも OS もアプリも当時を知るのに**紙媒体の方が永く残る**のを目の当たりにしています。ここでは、これまでのインタフェースを知る手掛かりとなる本を紹介します。

2011年頃の当時のインタフェースを知るには、同年に出版された『iPhoneアプリ設計の極意　思わずタップしたくなるアプリのデザイン』[^tapworthy]という本が当時の制約の中でどのようにデザインしていくかに言及しています。スクリーンショットを見ると、今と比べると小さな画面の中で効果的なインタラクティブを追求する過程も見れます。副題が**『思わずタップしたくなるアプリのデザイン』**とあり、タップ中心の文化形成の起点にもなっています。そのタップしたくなるアプリの入口としてのアイコンについても詳細に触れています。最近出版された『iOSアプリアイコン図鑑』[^ios-app-icons]と合わせて読むとアイコンへの見方が深まります。

2015年にはそれまでのモバイルデザインのパターンをまとめた『モバイルデザインパターン ユーザーインタフェースのためのパターン集』[^mobile-design-pattern-gallery]も出版されています。現在も配信中のアプリも事例として掲載されていました。アップデートにより今ではもう見ることのできない当時のバージョンのものや、配信停止や対応 OS の関係でインストールや起動できなくなってしまったアプリのスクリーンショットを通してインタフェースのパターンを学ぶことができます。この本の序文からインタフェースの学習方法について引用します。

> 最善の学習方法は視覚的な例に触れることであると考えます。私は物書きであり、言葉が大好きです。望まれるなら、パターンとは何か、どのパターンを選ぶべきか、それぞれのパターンの違いは何かといった点についていくらでも言葉を書き連ねるでしょう。しかし、単にインタフェースをデザインしようとする人（言い換えるなら、1つのツールとしてパターンを学ぼうとおする人）にとって、言葉は明らかに不必要です。パターンを「感じ取る」ことができる程度の説明と、厳選されたさまざまな事例さえあれば、そのパターンを身につけて知識を強固なものにできるはずです。

2018年『UI GRAPHICS』[^ui-graphics]では、デザイナ視点での知見が詰まっています。
副題に**『成功事例と思想から学ぶ、これからのインターフェイスデザインとUX』**とあり、成功事例として当時のアプリのスクリーンショットも見ることができます。思想にも触れていて、この辺りは副題の通り、これからのインタフェースを考える上で有用な概念だと感じます。

当時を振り返ったり、この辺りの書物に触れると iPhone SDK[^iphone-sdk] の功績にはデザイナとプログラマの境界線を曖昧にしたというのが多いと感じています。それによって、相互に越境し合ってモバイルデザインが急速に発展していった印象を持っています。

[^tapworthy]: JOSH CLARK, Tapworthy DESIGNING GREAT iPHONE APPS, O'REILLY, 2011, [深津貴之 監訳, 武舎広幸・武舎るみ 訳, iPhoneアプリ設計の極意－－思わずタップしたくなるアプリのデザイン, オライリー・ジャパン, 2011]
[^ios-app-icons]: マイケル・フラルップ, iOSアプリアイコン図鑑, ホビージャパン, 2022
[^mobile-design-pattern-gallery]: Theresa Neil, Mobile Design Pattern Gallery Second Edition UI Patterns for Smartphone Apps, O'REILLY, 2015, [深津貴之 監訳, 牧野聡 訳, オライリー・ジャパン, 2015]
[^ui-graphics]: 安藤剛・水野勝仁・萩原俊矢・ドミニク・チェン・菅俊一・鹿野護・有馬トモユキ・渡邊恵太・須齋佑紀/津﨑将氏, 【新版】UI GRAPHICS 成功事例と思想から学ぶ、これからのインターフェイスデザインと UX, BNN, 2018 / 2015年刊行の同書籍の改訂版として出版されました
[^iphone-sdk]: 当初は iPad もなく、 iPhone のみをターゲットとしていた Apple から提供される Software Development Kit をそう呼んでいました。

## インタフェースを変えてみよう

ここからは実演として既成のインタフェースを変えてみましょう。

今日において多くの人がディスプレイをタッチスクロールして情報を見ていると思います。そのタッチスクロールのユーザ操作を取り除く UI の案を紹介します。

### ユースケース

例えば、あなたが電車に乗って、車内がそこそこ混んでいたとしましょう。入り口に留まり続けるのも同調圧力がそうさせません。車内の奥へ進み、右手で吊り革に掴まって立った状態です。徐にスマホを取り出して、 X(Twitter)[^x-twitter] のアイコンをタップします。気になるポストがなくて、左手で持つスマホの親指でスクロールして気になるポストを探しています。

片手でタッチスクロールするのは、大変です。実は、昔のスマホは今よりも小さくて、その操作が今に比べると楽でした。徐々に大きく重くなるスマホに人間の方が訓練されてあまり苦ではないという人もいるかもしれません。しかし、マシンインプットで考えてみましょう。ユーザが操作に慣れるのではなく、マシンの方に頑張ってもらう方法はないでしょうか？　片手でタッチスクロールせずに**タッチスクロールと同等のマシンインプットを実現する方法**は何かあるでしょうか？　片手で持つスマホを傾けたら、その傾斜に応じてスクロールしてみたらどうでしょうか？

### サンプルアプリ

タッチスクロールをセンサーを利用したスクロールに変えるサンプルアプリを用意しました。

<img alt="iOS ソースコードへの QR コード" style="float:right;margin-left:16px" width=80 src="./images_kawashima/qrcode-ios.png">

下記 URL または QR コードからソースコードを入手できます。ビルドして iPhone にインストールすることで実際に UI の触り心地を確かめることができます。

https://github.com/ykws/motion-scroll-ios

<img alt="Android ソースコードへの QR コード" style="float:right;margin-left:16px" width=80 src="./images_kawashima/qrcode-android.png">

下記 URL または QR コードからソースコードを入手できます。ビルドして Android にインストールすることで実際に UI の触り心地を確かめることができます。

https://github.com/ykws/motion-scroll-android

### サンプルアプリの解説
#### 傾きの検知を利用できるようにする
##### iOS

Core Motion[^core-motion] を利用して iPhone の傾きを取得できます。次のように `import` すれば使えるようになります。


```swift
import CoreMotion
```

##### Android

SensorManger[^sensor-manager] を利用して Android の傾きを取得できます。 Manifest ファイルでの宣言が必要です。

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

`CMMotionManager` の `stopAccelerometerUpdates`[^stop-accelerometer-updates] を呼ぶことで傾き検知を終了できます。 `deinit` などで呼ぶようにしましょう。

```swift
motionManager.stopAccelerometerUpdates()
```

##### Android

`SensorManager` の `unregisterListener`[^unregister-listener] を呼ぶことで傾きの検知を終了できます。 `onDispose` など呼ぶようにしましょう。

```kotlin
sensorManager.unregisterListener(listener)
```

#### ボタンと組み合わせてスクロールを実装する

無制限に傾きでスクロールするのは操作しづらいので、左下にボタンを配置します。ボタンをタップしている間はスクロールするように制御を変えます。ボタンをタッチした時の傾きを基準として、そこから手前に傾けると下に、奥に傾けると上にスクロールするようにしてみました。

##### iOS

```swift
motionManager.startAccelerometerUpdates(to: .main) { [weak self] (data, error) in
    guard let self = self,
          let data = data else { return }

    // ボタンをタッチした時の傾きを基準とする
    // 初期値を nil にしておくことで、開始時の傾きだけ取得して更新し、終了時に解放されるまでは更新しない
    if (baseAccelerationY == nil) {
        baseAccelerationY = data.acceleration.y
    }

    // 基準の軸から今の傾きの差分を求めて、任意の係数で補正します
    // 補正する数字を大きくするほど、小さな傾きで大きく動くようになります
    let calculatedOffsetY = (baseAccelerationY! - data.acceleration.y) * 300

    // 今時点の UITableView の Y軸のオフセットに加算します
    tableViewOffsetY += calculatedOffsetY

    // スクロール範囲での Y座標の最大値を求めます
    let maxY = tableView.contentSize.height - tableView.bounds.height + view.safeAreaInsets.bottom

    // Y座標が範囲内に収まるように補正します
    if (tableViewOffsetY < 0) { tableViewOffsetY = 0 }
    if (maxY < tableViewOffsetY) { tableViewOffsetY = maxY }

    // 更新された Y座標を UITableView に反映します
    tableView.setContentOffset(CGPoint(x: 0, y: tableViewOffsetY), animated: true)
}
```

##### Android


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

[^x-twitter]: https://about.twitter.com/ja
[^core-motion]: https://developer.apple.com/documentation/coremotion
[^sensor-manager]: https://developer.android.com/guide/topics/sensors/sensors_overview
[^motion-manager]: https://developer.apple.com/documentation/coremotion/cmmotionmanager
[^start-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616148-startaccelerometerupdates
[^accelerometer-update-interval]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616135-accelerometerupdateinterval
[^register-listener]: https://developer.android.com/reference/android/hardware/SensorManager#registerListener(android.hardware.SensorEventListener,%20android.hardware.Sensor,%20int)
[^sensor-event-listener]: https://developer.android.com/reference/android/hardware/SensorEventListener
[^stop-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616138-stopaccelerometerupdates
[^unregister-listener]: https://developer.android.com/reference/android/hardware/SensorManager#unregisterListener(android.hardware.SensorEventListener)

## おわりに

今回紹介した案は、当たり前になっているけどユーザ操作量が多く、それをなくす、もしくは減らすことはできないかというユースケースに取り組みました。この方法がベストだとかそういうことを言いたいのではなく、当たり前になっている UI も変えることが可能であり、それをプログラマ[^designer-is-programmer]は**すぐに試すことができる**ということを伝えたかったです。

スマホには他にもセンサーが搭載されていて、 Apple は CoreMotion を Android は SensorManager といった形で、様々な機能を開放しています。

現代では、チーム開発が主流となっていて、 UI/UX はデザイナや顧客が考えるケースが増えていると思います。ですが、スマホの特徴を活かして、**マシンにユーザを従わせるのではなく、ユーザにマシンが従うようにシステムを設計・構築するのはプログラマの役目**だと考えています。大規模なチーム開発では、インタフェースを設計・構築する役割が分散するケースが多く、何かを試行錯誤する機会を得るのは難しいこともあると思います。

そこでぜひ、小規模で、**可能なら一人で**モバイルアプリのインタフェースの設計と構築を、 NoUI やマシンインプットの観点で再設計する機会を、一人でも多くの人が増えてほしいと思っています。そして来るべき visionOS の時代に備えたインタフェースを一緒に模索できたらと思います。そのための基礎体力として、既成のスマホのインタフェースにおいても十分に改良の余地があり、拡張または再構築していく体験が重要になってくると思っています。

### ヒント
今後、スマホの可能性を拡張または再構築するためのヒントを載せておきます。

- スマホに搭載されているセンサーにはどんなものがあるか調べてみよう
- SDK が提供している API にはどんなものがあるか調べてみよう
- 面倒だったのに慣れてしまった操作としてどんなものがあるか思い出してみよう
- もし画面がなかったら（あるいは目が見えない人に）どんな表現で情報を伝えることができるか考えてみよう

スマホの可能性を拡張または再構築するということは、**新しいインタフェースを構築すること**でもあります。一緒に新しいインタフェースを作っていきましょう。

[^designer-is-programmer]: ここでのプログラマはデザイナも含みます。 iPhone SDK がデザイナとプログラマの境界を曖昧にしたので、プログラミングを行うデザイナもプログラマと敬意を込めて呼んでいます。
