---
title: "Docker for Mac がバージョンアップ後に不安定な場合"
emoji: "🐳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Docker]
published: true
---

Docker for Mac を 2.4.0.0 にアップグレードしたところ、急にコンテナが不安定になりました。

以前のバージョンではそのような現象は発生していなかったので、 release notes から過去の stable version 2.3.0.5 にダウングレードして一旦落ち着いた様子。

https://docs.docker.com/docker-for-mac/release-notes/

## 関連しそうな情報
自分の環境では gRPC FUSE を無効にしても解消されませんでした。

* [Docker for Mac 2.4.0.0 の Issues](https://github.com/docker/for-mac/issues?q=is%3Aopen+is%3Aissue+label%3Aversion%2F2.4.0.0) 
* [Docker for Mac 2.4.0.0 でファイルの変更が反映されないとき](https://qiita.com/unchi/items/7929d254d4db5d34f015) 
