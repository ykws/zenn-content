---
title: "visionOS に備える"
emoji: "🕶️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios]
published: true
publication_name: yumemi_inc
---

# はじめに

iOS/iPadOS アプリを visionOS 向けにビルドすることで、VisionPro 上でも動かすことができます。しかし、今までの OS と visionOS では違いがあり、アプリの再設計が必要です。勉強会に参加して、visionOS に備える上で、キーとなりそうな項目をチェックリストとしてまとめました。

## 参加した勉強会
visionOS Developer Community Vol.1 に参加しました。
https://visionos.connpass.com/event/292434/

勉強会についてのポストが以下にまとめられています。
https://togetter.com/li/2261160

勉強会では、書籍「**visionOS デベロッパーへの道**」を中心に行われました。この本は技術書典マーケットで購入できます。
https://techbookfest.org/product/cA3PMcjbumehPUriiECzgt?productVariantID=wzGMbGj0KctCh32Wr2bqRW

:::message
後述する公式のガイド全てに目を通せていない段階です。勉強会に参加して自分が特に必要だと感じた点をここでは取り上げています
:::

# visionOS に備えるチェックリスト
- **Volume** の概念を抽出する
- **Landscape** に対応する
- **Day/Night** に対応する
- ジェスチャーに対応する
- アイディアを形にする

## Volume の概念を抽出する

visionOS の新しい概念として **Volume**[^1-1-2] があります。

Volume は、おそらく日本で一番よくイメージされる音量調整とは全く関係ありません。体積を持つものとしてイメージしてください。公式サイトでは、立方体の絵に添えられて次のように説明されています。

> Add depth to your app with a 3D volume. Volumes are SwiftUI scenes that can showcase 3D content using RealityKit or Unity, creating experiences that are viewable from any angle in the Shared Space or an app’s Full Space.[^vision-os]

visionOS の世界で、どの角度からでも表示できる体験を作成するのが Volume です。

visionOS Hello World[^vision-os-hello-world] の地球の部分が Volume です。アプリの中で Volume に相当しそうな部分を抽出しておきます。

## Landscape に対応する

Portrait 固定のアプリを VisionPro 上でそのまま表示するのは効果的ではありません。

書籍に視野角についてわかりやすい図が掲載されていました[^2-1-1]。それによると左右に 60度、上下に 30度の範囲にメインコンテンツを収めることが目安となっていて、縦方向の視野の範囲は、横方向の視野に比べて半分ほどになっています。このことから、Portrait 固定のアプリをそのまま VisionPro に持ち込むことは可能でも視線移動の負担になり、体験として好ましくないものになると予想されます。

後述する公式のガイドにも全ての向きをサポートせよと記載があります。

> Support all interface orientations whenever possible[^supported-interface-orientations]

年々縦方向に長くなるデバイスに対するアプリの表現方法として、ハーフモーダルやウィジェットが出始めていて、VisionPro の空間にアプリを表現する上でも有効になるであろうと感じています。

Galaxy Z Flip シリーズ[^galaxy-z-flip]では、折り畳まれることで物理的にも表示できる範囲が半分になります。このデバイスに対してアプリをどうやって表現するか考えるのもヒントになりそうです。

## Day/Night に対応する

カラー設計は、ダークモードとは異なります。visionOS の調整を前提とする必要があります[^2-2-3]。

visionOS の Simulator には **Simulated Scenes Day/Night** が用意されていて、 Day/Night を切り替えて、 visionOS 上でどのように反映されるのか確認できます。これを活用すると良いです。

## ジェスチャーに対応する

VisionPro にはコントローラは付属しないため、ジェスチャーによる操作を組み込む必要があります。

残念ながら、Simulator では確認できません。試したい場合は、ラボを活用できます[^4-1]。

https://developer.apple.com/visionos/labs/

ラボ予約前の注意事項として、矯正視力の人は**眼鏡作成の処方箋が必要**になります。眼科で眼鏡用の処方箋を希望して受診するとスムーズなようです。 VisionPro 用にという説明は省いても支障はないようです。逆にその説明を眼科にすると、余計な混乱を招いてスムーズに処方箋を発行してもらえない可能性もあります。

ジャスチャーを利用したサンプルコードは用意されています。

https://developer.apple.com/documentation/visionos/happybeam

## アイディアを形にする

MetaQuest[^meta-quest] 等のすでに発売されている VR デバイスと Unity[^unity] で先行開発が有力です。

Unity で作成したアプリも visionOS で動かすことは可能になる想定です。Apple は visionOS 向けに Unity をサポートすると宣言しているので、Unity で開発しておいて、visionOS 向けにバイナリをビルドするといったフローを想定しています。

> Now you can use Unity’s robust and familiar authoring tools to create new apps and games or reimagine your existing Unity-created projects for visionOS. [^vision-os-unity]

Unity からも visionOS 向けのガイドが用意されています。

https://create.unity.com/spatial

# 公式のガイド
Apple から公式のガイドも用意されています。
visionOS で動作する技術要件を始め、アンチパターンの紹介も含めてアプリデザインガイドが提示されています。

https://developer.apple.com/documentation/visionos/bringing-your-app-to-visionos
https://developer.apple.com/design/human-interface-guidelines/designing-for-visionos

# おわりに

既成のアプリを VisionPro でも動かしたいという要望が出てくると予想します。その時点でアプリの再設計をするのでは、リリースまでに多くの時間と労力が必要となり、希望に応えるのは困難になるとも予想します。仮にそのまま VisionPro に取り込めるからと安易に持ち込んでしまうと体験が損なわれる可能性を危惧しています。そうならないように Apple の審査が厳しくなるとも思っています。そこで先々の VisionPro 対応も視野に入れているのあれば、今回のチェックリストを元に先行して対応を検討し始めることをおすすめしたいです。特に Volume や Landscape 対応は表面的なレイアウトの課題ではなく、コンテンツをどのように捉えて表現するのかを、今後より求められていくと感じています。この考えは、ディスプレイの領域が大きくなっている iOS/iPadOS にも活かされると思っています。

[^1-1-2]: 「visionOS デベロッパーへの道」1.1.2 Volume
[^vision-os]: https://developer.apple.com/visionos/
[^vision-os-hello-world]: https://developer.apple.com/documentation/visionos/world#Declare-a-volumetric-window-for-the-globe
[^2-1-1]: 「visionOS デベロッパーへの道」2.1.1 視野の内側に収まるようにデザイン
[^supported-interface-orientations]: https://developer.apple.com/documentation/visionos/making-your-app-compatible-with-visionos#Audit-your-interface-code
[^galaxy-z-flip]: https://www.samsung.com/jp/smartphones/galaxy-z-flip5/
[^2-2-3]: 「visionOS デベロッパーへの道」2.2.3 テキストカラーは白がデフォルト
[^4-1]: 「visionOS デベロッパーへの道」4.1 visionOS Developer Lab に行ってきた
[^meta-quest]: https://www.meta.com/jp/quest/
[^unity]: https://unity.com/
[^vision-os-unity]: https://developer.apple.com/visionos/
[^unity-vision-os]: https://create.unity.com/spatial
