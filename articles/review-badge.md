---
title: "レビューコメントにメタ情報を持たせよう"
emoji: "📛"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [chromeextension,github,bitbucket]
published: true
publication_name: yumemi_inc
---

# はじめに

レビューコメントにメタ情報を持たせるためにラベルバッジをつけるということをやっています。

2021年くらいから以下の記事を影響を受けて真似し始めました。

:::details Draft から1年以上
この記事を公開しようと思って1年以上寝かせてしまいました
https://github.com/ykws/zenn-content/commit/3adbcce352dc04f1bd4785bfaad2f5ecc02f78f6
:::

https://qiita.com/iganin/items/aee297eade84849cc9cd

やってみて、自分にあった表現に見直したのでその紹介です。（カラーコードも見直しています）
Text Blaze の利用手順は上の記事で詳しく紹介されているのでここではしません。

レビューコメントにメタ情報を含めるのは、コメントを読む側はもちろん、コメントをする時点で、どういったリアクションを期待するのか、これは修正必須なのか、それともそうではないのか、期待する内容が曖昧なコメントを防ぐことができるので、レビューのやり取りがやりやすくなる効果があると思って取り込んでいます。

# 自分の設定内容

## GitHub

| badge | 期待するリアクション | 意味 | Text Blaze - shortcut | Text Blaze - content |
|-------|------|------|------|------|
| ![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg) | 回答必須 | 確認 | `/ask` | `![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg)` |
| ![must-badge](https://img.shields.io/badge/review-must-red.svg) | 修正必須 | この対応がされていないと Approve できない | `/must` | `![must-badge](https://img.shields.io/badge/review-must-red.svg)` |
| ![imo-badge](https://img.shields.io/badge/review-imo-orange.svg) | 修正任意 | こういう表現もあります | `/imo` | `![imo-badge](https://img.shields.io/badge/review-imo-orange.svg)` |
| ![nits-badge](https://img.shields.io/badge/review-nits-green.svg) | 修正任意 | 細かい指摘 | `/nits` | `![nits-badge](https://img.shields.io/badge/review-nits-green.svg)` |
| ![next-badge](https://img.shields.io/badge/review-next-blueviolet) | 修正不要 | 今後の改善点 | `/next` | `![next-badge](https://img.shields.io/badge/review-next-blueviolet)` |
| ![memo-badge](https://img.shields.io/badge/review-memo-lightgrey) | - | コード理解・解説のためのメモ書き | `/memo` | `![memo-badge](https://img.shields.io/badge/review-memo-lightgrey)` |
| ![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg) | - | 良い点 | `/good` | `![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg)` |

## BitBucket

BitBucket のレビューコメントでは GitHub とは異なるレンダリングを採用していて、 `height` を指定する必要があります。

| badge | Text Blaze - shortcut | Text Blaze - content |
|-------|------|------|
| ![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg)| `/b/ask` | `![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg){height=20}` |
| ![must-badge](https://img.shields.io/badge/review-must-red.svg) | `/b/must` | `![must-badge](https://img.shields.io/badge/review-must-red.svg){height=20}` |
| ![imo-badge](https://img.shields.io/badge/review-imo-orange.svg) | `/b/imo` | `![imo-badge](https://img.shields.io/badge/review-imo-orange.svg){height=20}` |
| ![nits-badge](https://img.shields.io/badge/review-nits-green.svg) | `/b/nits` | `![nits-badge](https://img.shields.io/badge/review-nits-green.svg){height=20}` |
| ![next-badge](https://img.shields.io/badge/review-next-blueviolet) | `/b/next` | `![next-badge](https://img.shields.io/badge/review-next-blueviolet){height=20}` |
| ![memo-badge](https://img.shields.io/badge/review-memo-lightgrey) | `/b/memo` | `![memo-badge](https://img.shields.io/badge/review-memo-lightgrey){height=20}` |
| ![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg) | `/b/good` | `![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg){height=20}` |


# 解説
## suggestion を除外
冒頭の記事に含まれていた `suggestion` はあまり使わなかったので、除外しました。
Code Suggestion を利用する時、レビューコメントの段階として、以下があり、どのケースにおいても提案（＝suggestion）なので、 `suggestion` 単独で利用するのは相応しくないと感じました。

- must: 推奨されているので必ず修正して欲しい場合
- imo: 推奨されているので自分ならこちらで書く場合
- nits: 細かいけど、単に推奨されている書き方を紹介する場合

## ask のカラー変更
`blue` だと `suggestion` の色と被っていて避けることにしました。（除外したので色の重複は気にしなくても良くなりましたが）
回答が得られないと判断が難しく、コードレビューにおける**意味の理解と把握**において重要であることから、カラーコードは黄金の `yellowgreen` を選択しました。 Googleのソフトウェアエンジニアリングから学ぶコードレビューの記事で**コードレビューそのもの**について詳しく書いています。

> コードレビューは、ある変更がより広い対象にとって理解可能かどうか試す最初の試練である

https://zenn.dev/yumemi_inc/articles/google-code-review#%E7%90%86%E8%A7%A3%E3%81%A8%E6%84%8F%E5%91%B3%E3%81%AE%E6%8A%8A%E6%8F%A1

## next を追加
レビューをしていて、この Pull Request のスコープ外だけど、改善点として気づいたことをコメントとして残します。
今回修正不要だけど、改善点であることを明確にし、このラベルバッジのコメントを Issue として作成すると対応漏れの防止にもつながります。

## memo を追加
コード理解や解説のためにレビューコメントを残すのが効果的なケースがあります。
セルフレビューだったり、他のレビュワーや、後で自分でレビューを見返すシーンだったりで相互理解の助けになります。そのような意味合いを他のレビューコメントと区別するために利用します。ただ、 memo に関しては、絵文字の 📝 がわかりやすいので、バッジラベルより絵文字で代替することが多いです。

## good を追加
レビューをしていて、ここは特に良いと思った点をコメントするようにしています。
単に褒めているだけという意図を明確にするためにこのバッジが目印になります。

# おわりに

ここまで Chrome Extension を前提に書いてきましたが、 GitHub に限定するなら、 Saved replies も活用でき、以下の記事で紹介されています。ここで紹介したラベル付けとも異なり、考え方の参考になるので、合わせて読んでもらえたらと思います。

https://zenn.dev/hacobell_dev/articles/code-review-comment-prefix
