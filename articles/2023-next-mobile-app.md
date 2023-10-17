---
title: "これからのモバイルアプリのカタチ"
emoji: "🕶️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [ios]
published: false
---

---
class: content
---

<div class="doc-header">
  <h1>これからのモバイルアプリのカタチ</h1>
  <div class="doc-author">川島慶之</div>
</div>

これからのモバイルアプリのカタチ
==

<img alt="Zenn への QR コード" style="float:right;margin-left:6px" width=80 src="./images_kawashima/grcode-zenn.png">

本記事は Zenn にも同じ内容を公開しています。出版後に加筆、修正がある場合は Zenn の記事で確認できます。

https://zenn.dev/yumemi_inc/articles/2023-next-mobile-app

---

## はじめに

筆者は2004年からモバイルアプリケーションプログラマとして携帯電話向けのアプリケーション開発に携わってきました。そこから20年経過しようとしています。これまではタッチスクリーンを主軸にモバイルアプリは発展してきました。この記事では、これからのモバイルアプリに対する考え方を共有します。
2023年 Apple[^apple] から visionOS[^vision-os] が発表されました。空間コンピューティングという言葉で示されるように、 visionOS では iOS での固定されていた画面単位という概念を大きく変えることでしょう。この記事では、 visionOS が iOS から変えていく具体的な内容について踏み込めていません。これから変わっていくであろうインタフェース[^interface]に対する考え方について述べていきます。

[^apple]: https://www.apple.com/jp/
[^vision-os]: https://developer.apple.com/jp/visionos/ https://developer.apple.com/documentation/visionOS
[^interface]: インタフェースの表記は、本記事では左記に統一します。書籍の名前や引用はその表記に従うため、その点において記事中の表記のぶれが発生することがあります。

### 対象者
- モバイルアプリケーションプログラマ
- モバイルアプリケーションデザイナー

## インタフェースとは

インタフェースとは何でしょうか？

何かと何かをつなぐ時に、その間を仲介するものをインタフェースと呼んでいます。何かと何かなので、モノとモノ、例えば、電源プラグとコンセントの関係もインタフェースになります。 UI と呼ぶ時は、 User Interface の略称なので、人に対して何かをつなぐシーンに限定されます。

モバイルアプリにおけるインタフェースは何でしょうか？

スマホ[^smart-phone]を使う人とスマホの間にあるものは全てインタフェースになります。今は主に画面です。ガラケー[^feature-phone]の頃は出力しかできなかった画面が、スマホ以降、出力と入力の両方の役割を担い、その大きさは年々大きくなっていっています。画面以外では、下記のようにユーザに対して機能を提供するものがあります。

- マイクとスピーカーによる音声の入出力
- 位置情報の送信
- カメラによる映像の入力（顔認証含む）
- 指紋認証
- 電源ボタン
- マナーモードボタン
- 音量調整ボタン

スマホの画面インタフェースの特徴として、自由にインタフェースを変更・配置できるため、箇条書きで示したハードに依存する固定化された役割ではなく、様々な表現と役割を担っています。

visionOS を見た時に今のインタフェースの主役であるこの画面の役割が変わるんだと思いました。もちろん、 iOS アプリの延長として visionOS の空間に再配置することは可能ですが、変えたいという思いが強いです。

これからのモバイルアプリのインタフェースを考えるにあたって、人を介さずにやり取りできる観点も持っておきたいので、 UI に限定されない場面では、インタフェースと表記します。

[^smart-phone]: スマートフォンの略称として、本記事では統一してそう呼ぶことにします。
[^feature-phone]: ガラパゴス・ケータイの略。スマートフォン登場後にそれと区別するために、スマートフォン登場前の携帯電話をそう呼んでいます。フィーチャーフォンと呼ばれることもあります。ガラケーでは、物理的なハードキーが付属しており、ボタンで入力を行っていました。

### これからのインタフェース

これからのインタフェースを考える時に2015年に出版された『さよなら、インタフェース』[^no-ui]という本の紹介をします。副題が『脱「画面」の思考法』とあり、タイトルでのインタフェースは画面を指していると思って良いです。この本の中で印象的な部分を引用します。

[^no-ui]: Golden Krishna, THE BEST INTERFACE IS NO INTERFACE, BNN, 2015, [武舎広幸 監訳, 武舎るみ 訳, さよなら、インタフェース　脱「画面」の思考法, BNN, 2015]

> 本書の中で、筆者はユーザーインプットではなく、マシンインプットを考えろと言う。ユーザーが何かを入力するのではなく、マシンがさまざまな情報を自動的に取得し、それを元にユーザーの必要なものを提供するのがこれからのインターフェースだと。

『さよなら、インタフェース』の帯には「時代は、NoUIへ。」と掲げられています。これを見た時に、次に来ると思ったのが、2015年でした。当時、 Alexa[^alexa] の登場もあって音声操作が主流になる可能性も感じていました。あれから8年経過しましたが、 NoUI　どころかアプリの画面数もユーザ入力も増加している印象です。その大きな要因がモバイルアプリ市場の拡大と大規模開発化してしまった点にあると思います。2011年頃のモバイルアプリ開発の黎明期と比べて、 2023年の現在は、チームで開発することが圧倒的に増えました。意図的にチームの規模を小さくして、極端には一人でのアプリケーション開発を制約とすることで、その限られたリソースから必然のアイディアとして、 NoUI やマシンインプットを考えるというキーワードは必要不可欠になっていくと考えています。

[^alexa]: https://www.amazon.co.jp/meet-alexa/b?ie=UTF8&node=5485773051

### これまでのインタフェース

これまでのインタフェースの変遷を知るのに、モバイルアプリというのは実にその多くが失われてしまいました。デバイスも OS もアプリも当時を知るのに紙媒体の方が永く残るのを目の当たりにしています。ここでは、これまでのインタフェースを知る手掛かりとなる本を紹介します。

2011年頃の当時のインタフェースを知るには、同年に出版された『iPhoneアプリ設計の極意　思わずタップしたくなるアプリのデザイン』[^tapworthy]という本が当時の制約の中でどのようにデザインしていくかに言及しています。スクリーンショットを見ると今と比べると小さな画面の中で効果的なインタラクティブを追求する過程も見れます。副題が『思わずタップしたくなるアプリのデザイン』とあり、タップ中心の文化形成の起点にもなっています。

2015年にはそれまでのモバイルデザインのパターンをまとめた『モバイルデザインパターン ユーザーインタフェースのためのパターン集』[^mobile-design-pattern-gallery]も出版されています。現在も配信中のアプリも事例として掲載されていますが、アップデートにより今ではもう見ることのできない当時のスクリーンショットを通してパターンの分析を学ぶことができます。

2018年に2015年に刊行された書籍の新版として出版された『UI GRAPHICS』[^ui-graphics]では、デザイナー視点での知見が詰まっています。
副題に『成功事例と思想から学ぶ、これからのインターフェイスデザインとUX』とあり、成功事例として当時のアプリのスクリーンショットも見ることができます。思想にも触れていて、この辺りは副題の通り、これからのインタフェースを考える上で有用な概念だと感じます。

当時を振り返ったり、この辺りの書物に触れると iPhone SDK[^iphone-sdk] の功績にはデザイナーとプログラマの境界線を曖昧にしたというのが多いと感じています。それによって、相互に越境し合ってモバイルデザインが急速に発展していった印象を持っています。

[^tapworthy]: JOSH CLARK, Tapworthy DESIGNING GREAT iPHONE APPS, O'REILLY, 2011, [深津貴之 監訳, 武舎広幸・武舎るみ 訳, iPhoneアプリ設計の極意－－思わずタップしたくなるアプリのデザイン, オライリー・ジャパン, 2011]
[^mobile-design-pattern-gallery]: Theresa Neil, Mobile Design Pattern Gallery Second Edition UI Patterns for Smartphone Apps, O'REILLY, 2015, [深津貴之 監訳, 牧野聡 訳, オライリー・ジャパン, 2015]
[^ui-graphics]: 安藤剛・水野勝仁・萩原俊矢・ドミニク・チェン・菅俊一・鹿野護・有馬トモユキ・渡邊恵太・須齋佑紀/津﨑将氏, 【新版】UI GRAPHICS 成功事例と思想から学ぶ、これからのインターフェイスデザインと UX, BNN, 2018
[^iphone-sdk]: 当初は iPad もなく、 iPhone のみをターゲットとしていた Apple から提供される Software Development Kit をそう呼んでいました。

## インタフェースを変えてみよう

ここからは実演として既成のインタフェースを変えてみましょう。

今日において多くの人がディスプレイをタッチスクロールして情報を見ていると思います。そのタッチスクロールのユーザ操作をなくしてみる UI の案を紹介します。

### ユースケース

例えば、あなたが電車に乗って、車内がそこそこ混んでいたとしましょう。入り口にいるのも同調圧力がそうさせません。車内の奥へ進み、右手で吊り革に掴まって立った状態です。徐にスマホを取り出して、 X(Twitter)[^x-twitter] のアイコンをタップします。気になるポストがなくて、左手で持つスマホの親指でスクロールして気になるポストを探しています。

片手でタッチスクロールするのは、大変です。実は、昔のスマホは今よりも小さくて、その操作が今に比べると楽でした。徐々に大きく重くなるスマホに人間の方が訓練されてあまり苦ではないという人もいるかもしれません。しかし、マシンインプットで考えてみましょう。ユーザが操作を覚えるのではなく、マシンの方に頑張ってもらう方法はないでしょうか？　片手でタッチスクロールせずに**タッチスクロールと同等のマシンインプットを実現する方法**は何かあるでしょうか？　片手で持つスマホを傾けたら、その傾斜に応じてスクロールしてみたらどうだろうかと思いつきました。

### サンプルアプリ

iPhone アプリのタッチスクロールをセンサーを利用したスクロールに変えるサンプルアプリを用意しました。

<img alt="ソースコードへの QR コード" style="float:right;margin-left:16px" width=80 src="./images_kawashima/qrcode-github.png">

下記 URL または QR コードからソースコードを入手できます。ビルドして iPhone にインストールすることで実際に UI の触り心地を確かめることができます。

https://github.com/ykws/MotionScrollApp


#### Core Motion を使う

Core Motion[^core-motion] を利用してデバイスの傾きを取得できます。次のように `import` すれば使えるようになります。


```swift
import CoreMotion
```

#### 傾き検知の開始

`CMMotionManager`[^motion-manager] の `startAccelerometerUpdates`[^start-accelerometer-updates] を呼ぶことで傾き検知を開始できます。クロージャに傾きが更新された時の処理を書きます。更新の頻度は `accelerometerUpdateInterval`[^accelerometer-update-interval] で指定できます。


```swift
let motionManager = CMMotionManager()

// 更新の頻度を指定できます
motionManager.accelerometerUpdateInterval = 0.1

motionManager.startAccelerometerUpdates(to: .main) { [weak self] (data, error) in
    // 傾きが更新された時の処理
}
```

#### 傾き検知の終了

`CMMotionManager` の `stopAccelerometerUpdates`[^stop-accelerometer-updates] を呼ぶことで傾き検知を終了できます。 `deinit` などで呼ぶようにしましょう。

```swift
motionManager.stopAccelerometerUpdates()
```

#### ボタンと組み合わせてスクロールの実装

無制限に傾きでスクロールするのは操作しづらいので、左下にボタンを配置します。ボタンをタップしている間はスクロールするように制御を変えます。ボタンをタッチした時の傾きを基準として、そこから手前に傾けると下に、奥に傾けると上にスクロールするようにしてみました。

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

[^x-twitter]: https://about.twitter.com/ja
[^core-motion]: https://developer.apple.com/documentation/coremotion
[^motion-manager]: https://developer.apple.com/documentation/coremotion/cmmotionmanager
[^start-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616148-startaccelerometerupdates
[^accelerometer-update-interval]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616135-accelerometerupdateinterval
[^stop-accelerometer-updates]: https://developer.apple.com/documentation/coremotion/cmmotionmanager/1616138-stopaccelerometerupdates

## デバイスの可能性を拡張してみませんか

今回紹介した案は、当たり前になっているけどユーザ操作量が多く、それをなくす、もしくは減らすことはできないかという実演でした。この方法がベストだとかそういうことを言いたいのではなく、当たり前になっている UI も変えることが可能であり、それをプログラマ[^designer-is-programmer]はすぐに試すことができるということを伝えたかったです。

Apple からは他にもセンサーが搭載されていて、デバイスのリソースを広く開放しています。

現代では、チーム開発が主流となっていて、 UI/UX はデザイナや顧客が考えるケースが増えていると思います。ですが、モバイルデバイスの特徴を活かして、マシンにユーザを従わせるのではなく、ユーザにマシンが従うようにシステムを設計・構築するのはプログラマの役目だと考えています。大規模なチーム開発では、インタフェースを設計・構築する役割が分散するケースが多く、何かを試行錯誤する機会を得るのは難しいと思います。

ぜひ、小規模で、可能なら一人でモバイルアプリのインタフェースの設計と構築を、 NoUI やマシンインプットの観点で再設計する機会を一人でも多くの人が増やしてほしいと思っています。そして来るべき visionOS の時代に備えたインタフェースを次の時代で一緒に模索できたらと思います。そのための基礎体力として、既成の iOS のインタフェースにおいても十分に改良の余地があり、再構築していく体験が重要になってくると思っています。

[^designer-is-programmer]: ここでのプログラマはデザイナも含みます。 iPhone SDK がデザイナとプログラマの境界を曖昧にしたので、プログラミングを行うデザイナをプログラマと敬意を込めて呼んでいます。
