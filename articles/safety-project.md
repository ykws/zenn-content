---
title: "プロジェクトに浅瀬を作る"
emoji: "🏖️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [心理的安全]
published: true
publication_name: yumemi_inc
---

# はじめに

プロジェクトに参加しているメンバーがうまく環境に適用できずに離脱することがあり、ともすれば、身体を壊してしまうケースもあります。これは新規メンバーに限定されず、既存のメンバーでも、プロジェクトや本人の状況、その役割が変われば発生し得ると思っています。

そういったことを回避できた状態を想像した時にプロジェクトに浅瀬があったら良いのではというイメージからこの言葉が浮かんだのだと思います。2年ほど前のメモ書きにこのタイトルが残されていて、今見直した時にすごくしっくり来ました。

:::details メモ書きを発見したツイート
@[tweet](https://twitter.com/ykws__/status/1505821070200217602?s=21)
:::

この「プロジェクトに浅瀬を作る」とは、どういうことなのか、改めて深堀したいと思います。

# どういうこと？

- 溺れないようにするのが目的
- 監視員が必要のない状態が理想

## 溺れないようにするのが目的
溺れるというのは、闇雲に時間がかかってしまい心身ともに疲弊してしまうイメージです。不慣れなため必要な情報にアクセスできない、必要な操作がうまくできないなどが考えられます。

## 監視員が必要のない状態が理想
ずっと監視して問題が起きたら対処するといった、対処療法ではなく、環境を定期的に検査して維持し、放任しても大怪我をしない状態をイメージしています。

つまり、**気軽に失敗でき、心理的にも論理的にも安全な状態を用意する**、ということをしたいです。

# 何をする？

- 深いところを予め調査してそこが深いとわかるように目印をつける
- 安全に深いところに行くための訓練を行う
- 浅瀬で遊ぶことができる

# 具体的には？

- ドキュメントを整備する
- スクリプトを作成する
- ペアワークを実施する
- テストコードを追加する
- コードグラフを用意する
- オンボーディングを用意する

## ドキュメントを整備する

- 機能仕様とその変更理由
- 設計方針とその採用理由
- 抱えている課題と障壁となる事項

### 機能仕様とその変更理由
特に頻繁に変更される機能は必然的に触れる機会も多くなり、壊してしまう可能性が高いため、変更する際に、その機能がどういった仕様なのかわかるのと、また過去の変更理由も知れることで、これからの変更が妥当であるか比較可能であると事故が起きにくくなります。

### 設計方針とその採用理由
設計方針を知っておくと、コードの理解と変更がしやすいです。また、何を基準に設計方針、アーキテクチャを選択したのかわかっていると改善する際に見当違いな検討を避けられます。

### 抱えている課題と障壁となる事項
一般的に「ここは直した方が良い」ということでも既存の制約から当該プロジェクトにおいては実現が困難なことがわかると良いです。既存の制約が明文化されていれば、その制約を解消できた時に課題を改善できると判断がしやすいです。よく「直した方が良いところを教えて欲しい」とお願いしたりされたりすると思います。予めそういったリストと、なぜそれが直されていないのか理由があるはずなので、そういった既存の評価を踏まえた上で新たに検討した方が効率が良いです。歴史的背景が読み取れない状態での提案は可能ですが、障壁となる事案により却下されるので、提案した側のモチベーションが著しく下がってしまいます。

## スクリプトを作成する
繰り返しの作業において余計なミスを混入させないためにも、特に環境構築は参入時の障壁になるので手順の考慮が不要なスクリプトにまとまっているのが望ましいです。テスト用にプロダクトのアカウントやデータの作成・更新なども煩雑な作業になりがちなので、用意すると良いです。

## ペアワークを実施する
安全に深いところへ行くための訓練の役割もあります。既存のメンバーがどうやって作業するか見ることで学ぶ機会になります。また、新規メンバーがどうやって作業するか一緒に見ることでプロジェクトに浅瀬ができているか評価でき、この時に溺れそうならすぐに助けることもできます。

## テストコードを追加する
テストコードがない部分は危険な場所であり、仮に変更も不具合も発生しない箇所なら極論なくても問題はありません。頻繁に変更され、特に不具合による変更が発生しているなら、テストコードを追加します。歴史的な経緯からテストコードの追加が困難な場合は、テストがなくて危険な場所であることをドキュメントやコメントで明らかにします。

## コードグラフを用意する
テストコード追加の目安と、もしない場合の危険地帯の目印として以下についてグラフもしくはスコア化されてわかると良さそうです。

- 頻繁に変更されやすい箇所
- 不具合により変更が多発している箇所

## オンボーディングを用意する
プロジェクトをどうやって歩けば良いかわかるようにします。ここまでで挙げてきた具体例に対してのアクセスを提供し、気軽に失敗する機会を増やして、失敗したらどうリカバリできるかを体験できると良さそうです。あえて**気軽に失敗する**という表現を使っていますが、当該プロジェクトにおいて、通常失敗と捉えられる結果は、実は失敗ではなく、特定のオペレーションが発生する条件に過ぎないと再定義できると、心理的な安全を高くできたと言えるのではないでしょうか。

Googleのソフトウェアエンジニアリング[^1]では失敗を次のようにも捉えられているようです。

> Googleには、社員たちお気に入りのモットーとして「失敗は選択肢の1つである」というものがある。ときどき失敗するようなことがなかったとすれば、十分に革新的ではないか、十分にリスクを取っていないかのどちらかであるということが、広く認められているのだ。失敗は、次回の試みに向けての学習と改善のための、値千金の機会とみなされている

# ゴール

ゴールはありません。アジャイルサムライ[^2]からの引用と同じ特性のものだと思います。

> 「もうやり残りしたことはないし、何もかもわかった」と思った瞬間、君はもうアジャイルじゃなくなるってことだ

絶えず、検査と改善を行い、環境を維持するため、終わりがありません。ここで挙げた具体例に対しても実施して評価して更新していきます。

また、十分にできていると証明する方法もありません。

- 溺れないようにするため、仮に溺れている人がいなかったとしても、それが即ち「助けた」とは言えません
- 具体的にドキュメントなど用意されていて「助かった」というフィードバックがもしかしたら評価軸になるかもしれません
- あるいは、他のプロジェクトとの比較で、当該プロジェクトの健全さが差別化できるかもしれません

# おわりに

ここに記載したことは、参加しているプロジェクトによって対応にばらつきがあり、不十分な状態であると思っています。こうして言語化したことで、各プロジェクトに対して、どの取り組みを強化していくべきか評価して、推進していってみたいと思います。

# 他社の取り組み
- Ubie Discovery - 入社した会社にすばやく適応する[^3]

[^1]: [Googleのソフトウェアエンジニアリング――持続可能なプログラミングを支える技術、文化、プロセス](https://www.oreilly.co.jp/books/9784873119656/)
[^2]: [アジャイルサムライ――達人開発者への道](https://shop.ohmsha.co.jp/shopdetail/000000001901/)
[^3]: [WEB+DB PRESS Vol.127 - 特集3 入社した会社にすばやく適応する 事業構造，カルチャー，コードの把握](https://gihyo.jp/magazine/wdpress/archive/2022/vol127)
