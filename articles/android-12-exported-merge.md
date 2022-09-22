---
title: "Android12未対応のライブラリを取り込む"
emoji: "🍧"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [android,android12]
published: false
---

2022年の11月より Android アプリを更新する際は Android 12 対応が必須になります。

https://developer.android.com/google/play/requirements/target-sdk?hl=ja

この中でも **コンポーネントのエクスポートの安全性が改善されることの影響** で、この部分が未対応のライブラリを利用していると、ビルドは成功しますが、 Android 12 以降のデバイスにインストールしようとすると失敗します。ビルドのみの CI で安心していると発覚が遅くなるため要注意です。

インストール時のエラーは以下のような内容で出力されます。

```
Installation did not succeed.
The application could not be installed: INSTALL_PARSE_FAILED_MANIFEST_MALFORMED

xxx.yyy.zzz.AAA: Targeting S+ (version 31 and above) requires than an explicit value for android:exported be defined when intent filters are present
```

エラーの内容が非常に丁寧で何が起きているのか理解しやすいです。上の例では、 `xxx.yyy.zzz.AAA` が exported 属性が付与されていないと教えてくれています。

対応内容についてはドキュメントにも明記されている通りです。

> Android12以降をターゲットとするアプリに、インテントフィルタを使用するアクティビティ、サービス、またはブロードキャストレシーバが含まれている場合は、それらのアプリコンポーネントで `android:exported` 属性を明示的に宣言する必要があります。

https://developer.android.com/about/versions/12/behavior-changes-12?hl=ja#exported

さて、ここで変更可能なコードであれば上記に従って属性を明示的に宣言すれば良いですが、利用しているライブラリがソースコードを非公開にしている場合などは、対応のしようがありません。

しかし、こういった条件でも、複数のマニフェストファイルをマージする方法を Google は提供しており、対応が可能になっています。

> マージルールを XML 要素全体（指定マニフェスト要素内のすべての属性と、その要素のすべての子タグ）に適用するには、次の属性を使用します。
>
> `tools:node="merge"`
> 
> 競合がない場合、マージ競合ヒューリスティックに基づいて、このタグ内のすべての属性と、ネストされたすべての要素をマージします。これは、要素に対するデフォルトの動作です。

https://developer.android.com/studio/build/manifest-merge?hl=ja#node_markers

よって、今回の例として、 `xxx.yyy.zzz.AAA` ライブラリに対して以下のように指定することで exported 属性を付与した状態でマージしてくれるため Android12 に対応したものとして取り込むことが可能になります。この変更により Andorid12 以降のデバイスにインストール起動できることを確認できました。

```
<service
  android:name="xxx.yyy.zzz.AAA"
  android:exported="false"
  tools:node="merge" />
```

なお、 exported を true にするか false にするかは、 **アプリ コンポーネントに LAUNCHER カテゴリが含まれている** かどうかで判断します。

> アプリ コンポーネントに LAUNCHER カテゴリが含まれている場合は、android:exported を true に設定します。他のほとんどの場合は、android:exported を false に設定します。
