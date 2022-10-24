---
title: "iOSアプリで時間のかかる処理をする"
emoji: "🌈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios, swift, thread]
published: false
---

そもそもなぜスレッド制御が必要なのか

iOS アプリでは、画面をタップしたことを検知して画面の更新などを担当するメインスレッドというものがあります。単純なアプリにおいては、このメインスレッドを意識する必要はないです。もしアプリの中で、時間のかかる処理が発生した時に、メインスレッドだけだと、画面の更新が止まってしまいます。そこで、別のスレッドを用意し、その時間のかかる処理を任せることで、画面の更新を行いつつ、時間のかかる処理を並行して行い、処理が完了したら、その結果を画面に反映する、といったことが可能になっています。

それを実現するために、 GCD(=Ground Central Dispatch) があり、 Swift では DispatchQueue が用意されています。

時間のかかる処理を行いたい時に DispatchQueue.global.async のクロージャに渡すことでキューに処理を渡せます。この宣言で別のスレッドに切り替わるので、メインスレッドの処理を中断することを回避できます。
メインスレッドで画面の更新を行いたい時に DispatchQueue.main.async のクロージャに渡すことでキューに処理を渡せます。この宣言でメインスレッドに切り替わるので、画面の更新を行えるようになります。 iOS ではメインスレッド以外で画面の更新はできないためです。

# 参考文献
- [UI 関連の機能はメインスレッドで実行すること : Objective-C プログラミング](https://ez-net.jp/article/D9/pFIzkE5B/-augKeCpM4zZ/)
- [macOS/iOSスレッドプログラミング（ThreadとRunLoop）](https://qiita.com/cubenoy22/items/098a90133dfdc3f33ccc)
- [エキスパートObjective-Cプログラミング　-iOS/OS Xのメモリ管理とマルチスレッド-](https://book.impress.co.jp/books/3109)


