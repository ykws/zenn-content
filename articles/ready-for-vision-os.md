---
title: "visionOS に備える"
emoji: "🕶️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios]
published: false
---

勉強会の感想をまとめます
https://visionos.connpass.com/event/292434/

他の人が勉強会の様子をポストしているまとめ
https://togetter.com/li/2261160

- 本の紹介
https://techbookfest.org/product/cA3PMcjbumehPUriiECzgt?productVariantID=wzGMbGj0KctCh32Wr2bqRW

技術的にiOS, iPadOSアプリをvisionOS向けにビルドすることで表示可能ではある。
しかし、今までのOSとvisionOSの違いから次の点についてモバイルアプリの再設計が必要である。

- Landscape対応
  - Portrait固定のモバイルアプリをそのまま表示するのは効果的ではない
  - 視野角の話
- カラー設計
  - ダークモードとは異なる OS の調整を前提とする必要がある
  - Simulated Scenes Day/Night
- Unityでアイディアの確認をしておく
  - ARのアイディアを形にしてデバイスで確認するならMetaQuestなどとUnityで先行開発が有力
  - UnityベースでもvisionOSで動かすことは可能
- ジェスチャー対応
  - 実機なしで確認できない
    - 実機はラボ
    https://developer.apple.com/visionos/labs/
    - ラボ予約前に矯正視力の人は眼鏡作成の処方箋が必要
  - サンプルコードはある
  https://developer.apple.com/documentation/visionos/happybeam
