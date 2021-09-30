---
title: "レガシーな iOS アプリ開発の参考書籍"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,objective-c,uikit,test]
published: false
---

# 目的
時代は SwiftUI ですが、
UIKit + Objective-C でレガシーなアプリを作成、メンテナンスをする必要がある人向けに参考書籍をまとめます。

## iOS アプリ開発の歴史
かなり単純化して iOS アプリを構成する要素として、 以下のような大枠で捉えることができて、順にトレンドが移っています。

1. UIKit + Objective-C
2. UIKit + Swift
3. SwiftUI (=Swift)

最新のトレンドであれば、 Apple からチュートリアルが提供されているので、これがおそらくとてもわかりやすくて一番効果的だと思います。
https://developer.apple.com/tutorials/app-dev-training/

ただ、ここでは、初期のレガシーな構成を維持する必要性がある人向けなので、その情報を掘り起こしてみます。

# 前提

:::message alert
今回紹介する書籍は古いので、細かい部分は最新の情報と変わっている可能性が非常に高いです。
:::

必ず最新の公式ドキュメントと照らし合わせるのが良いです。

https://developer.apple.com/documentation/technologies?language=objc

後述の紹介で繰り返し述べますが、細部は変わってはいても、基本的な概念に関しては変わっていなくて、かつ最新の書籍や記事では得られない Objective-C のコード例が充実しているのでどれも参考になると判断しています。

# 書籍紹介
## UIKit 
https://book.impress.co.jp/books/1114101012

この本が出た当時から iOS の UI に関する SDK の基本的な部分は大きく変わっていなくて、それらを UIKit と呼んでいるのですが、その基本的な概念が学べます。コードの例は Objective-C です。

### iPhone SDK
https://www.oreilly.co.jp/books/9784873114170/

UIKit は iPhone SDK の一部で、 UIKit 以外の iPhone SDK の全体像を俯瞰できます。こちらもコードの例は Objective-C です。当時 Feature Phone では表現できなかった表現力を iPhone SDK がサードパーティに公開し、モバイルアプリの表現力が大きく変わりました。

## Objective-C
https://www.sbcr.jp/product/4797368277/

Objective-C の言語仕様そのものについて学べます。 iOS アプリの話は基本的に出てきません。 iOS アプリのコードリーディングの索引として利用すると、コードの理解の大きな助けになります。

## XCTest
https://www.shuwasystem.co.jp/book/9784798040899.html

iOS アプリのテスト自動化について、 XCTest という仕組みが利用できます。 iOS アプリのテストは Xcode に紐づいている部分も大きく、古い書籍なので、 UI 操作に関しては参考にするのは難しいです。最新の Xcode ではできない部分も存在しますが、 XCTest の基本的な部分は大きく変わっていなくて、基本的な概念が学べます。コードの例は Objective-C です。

## iPhone アプリ
https://www.ohmsha.co.jp/book/9784873115023/

当時とは様変わりした iPhone アプリですが、モバイルアプリそのものの設計の概念が学べます。具体的なコードは出てこないので、これを読んでアプリを開発できる即効性はないですが、モバイルアプリが何を目指すべきなのか教えてくれると思います。アプリの良し悪しを判断する軸ができるようになります。

:::message
iPhone アプリと iOS アプリは意図して表現を変えています。当時は iPhone アプリしかありませんでしたが、現在は iPad もあるので iOS アプリと表現することが多いです。ただ iPad は iPad OS なので、この表現も不適切かもしれません。
:::

# 4年前のまとめ
ちょうど4年前に似たようなまとめをしていました。
当時は、 Objective-C から Swift への移行の時期だった記憶があり、あのような選書になったのだと思います。

https://ykawashi7.hatenablog.com/entry/2017/09/30/015609

