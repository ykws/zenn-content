---
title: "プルリクエストへのコメントのラベルバッジ紹介"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [コードレビュー, pullrequest]
published: false
---

[プルリクエストへのコメントでは Text Blaze でラベルバッジをつけよう！](https://qiita.com/iganin/items/aee297eade84849cc9cd)

この記事を参考に真似してみました。
やってみて、自分にあった表現に見直したので紹介です。（カラーコードも見直しています）
Text Blaze の利用手順は上の記事で詳しく紹介されているのでここではしません。

# 一覧

| badge | 意味 | Text Blaze |
|-------|------|------|
| ![must-badge](https://img.shields.io/badge/review-must-red.svg) | 修正必須。この対応がされていないと Approve できない | `![must-badge](https://img.shields.io/badge/review-must-red.svg)` |
| ![imo-badge](https://img.shields.io/badge/review-imo-orange.svg) | 修正任意。自分ならこうするけど、どう？ | `![imo-badge](https://img.shields.io/badge/review-imo-orange.svg)` |
| ![nits-badge](https://img.shields.io/badge/review-nits-green.svg) | 修正任意。細かい指摘 | `![nits-badge](https://img.shields.io/badge/review-nits-green.svg)` |
| ![next-badge](https://img.shields.io/badge/review-next-blueviolet) | 修正不要、今後の改善点 | `![next-badge](https://img.shields.io/badge/review-next-blueviolet)` |
| ![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg) | 回答必須。確認 | `![ask-badge](https://img.shields.io/badge/review-ask-yellowgreen.svg)` |
| ![memo-badge](https://img.shields.io/badge/review-memo-lightgrey) | 回答不要。コード理解・解説のためのメモ | `![memo-badge](https://img.shields.io/badge/review-memo-lightgrey)` |
| ![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg) | 良い点 | `![good-badge](https://img.shields.io/badge/review-good-blightgreen.svg)` |

# 解説
## suggestion を除外
`suggestion` はあまり使わなかったです。
GitHub Code Suggestion を利用する時、レビューコメントの段階として、以下があり、どのケースにおいても提案（＝suggestion）なので、 `suggestion` 単独で利用するのは相応しくないと感じました。

- must
- imo
- nits

## next を追加
レビューをしていて、この Pull Request のスコープ外だけど、改善点として気づいたことをコメントとして残します。
今回修正不要だけど、改善点であることを明確にし、このバッジのコメントを Issue として作成すると対応漏れの防止にもつながります。

## ask のカラー変更
`blue` だと GitHub Code Suggestion の色と被っていて避けることにしました。
意味合いとしては、回答が得られないと判断が難しいので、警告のニュアンスに近けど、そこまで重くはないので、 `yellowgreen` にしています。

## memo を追加
コード理解や解説のためにコメントをしたくなります。
セルフレビューだったり、他のレビュワーや、後で自分でレビューを見返すシーンだったりで重宝します。
そのような意味合いを他のレビューコメントと区別するために利用します。

## good を追加
レビューをしていて、ここは特に良いと思った点をコメントするようにしています。
単に褒めているだけという意図を明確にするためにこのバッジが目印になります。

