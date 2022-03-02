---
title: "Xcode 13 で Vim mode を有効にする"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["xcode", "vim"]
published: true
---

# Xcode13 で Vim mode が導入されました
[Xcode](https://developer.apple.com/xcode/)

> Vim mode
Many common key combinations and editing modes familiar to Vim users are supported directly within the code editor, using the new bottom bar to show mode indicators.

[Xcode 13 Beta Release Notes](https://developer.apple.com/documentation/xcode-release-notes/xcode-13-beta-release-notes)

> Xcode 13 beta requires a Mac running macOS 11.3 or later.

なお、 Xcode13 を動かすには macOS 11.3 以上でよく、 [Monterey](https://developer.apple.com/macos/) にする必要はありません。

# 有効にするには
Editor > Vim Mode にチェックをつけます

![](https://storage.googleapis.com/zenn-user-upload/607c24684fc8-20220303.png)

## Xcode13 Beta 2 まで
[Xcode 13 Beta Release Notes](https://developer.apple.com/documentation/xcode-release-notes/xcode-13-beta-release-notes)

> Xcode 13 introduces Vim key bindings, emulating a vim experience in the source editor combined with the existing editor functionality. (3716281)
Enable Vim key bindings in Preferences, using the Enable Vim key bindings option in Text Editing > Editing.

Preferences > Text Editing > Editing > Vim key bindings のチェックボックスをオンにします。

![](https://storage.googleapis.com/zenn-user-upload/1a8bcc126b02862a42a7024d.png)

# 有効になると
Editor 下に Vim の表示が！
![](https://storage.googleapis.com/zenn-user-upload/83cae8b6710b25d26a72e581.png)

当時、 Preferences > Key Bindings にあると思って探して見つからなかったので、
「Xcode13 には導入するけど、まだ対応したとは言っていない」
Beta版だから未対応かと思ってしまいましたが、ちゃんと対応していました。

# XVim2 ありがとう
[Xcode 13 - Vim Supported. Thank you Xvim!](https://github.com/XVimProject/XVim2/issues/380)

これまで Xcode Vim をサポートしてくれた XVim に感謝の Issue が作成されています。
