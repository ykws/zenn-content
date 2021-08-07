---
title: "Delegateパターンで何が嬉しいか"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ios", "delegate"]
published: false
---

何か処理を書いていて、ある部分から先は後で好きに使ってもらえれば良い、ということがある。
こういう時に嬉しいのが Delegate パターンである。

## 例題

例えば、ゲームパッドについて考えてみよう。

そのゲームパッドで何のゲームをするかわからないけど、ボタンを押したら、どのボタンを押したかイベントを通知する仕組みがあれば、ゲームの方で、ボタンに対応した処理を実装すればできそうな気がする。

こういう時に Delegate パターンを使うと、ゲームパッドをこういった感じに表現できる。

```swift
protocol GamePadViewDelegate: AnyObject {
    // ゲームパッドで何かボタンを押したら、どのボタンを押したか通知する
    func gamePadView(_ gamePadView: GamePadView, didSelectButtonAt position: ButtonPosition)
}
```

ゲームパッドを利用する各画面では次のように通知に対して処理を実装するだけで、ボタンに応じた処理を実行することが可能になる。

```swift
extension ViewController: GamePadViewDelegate {    
    func gamePadView(_ gamePadView: GamePadView, didSelectButtonAt position: ButtonPosition) {
        // この画面で何かボタンを押した時の処理
    }
}
```

GamePad 側はボタンを押されたら通知することだけに集中しているので、画面ごとに処理が変わることなく、共通の View として利用できる。

通知した後のことは関心を持たないことで、Apple やサードパーティから提供される SDK などは Delegate パターンで通知処理だけを提供し、実装側で必要な通知を受け取って処理できるような仕組みができるようになっている。

## サンプルコード
https://github.com/ykws/GamePadExample

## まとめ


## Apple Delegate
Xcode でプロジェクトを新規作成した時に自動生成される AppDelegate, SceneDelegate はそれぞれ Apple から提供されている以下の Delegate を実装している。

- [UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
- [UIWindowsSceneDelegate](https://developer.apple.com/documentation/uikit/uiwindowscenedelegate)
