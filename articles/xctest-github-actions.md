---
title: "GitHub Actions で XCTest を実行する"
emoji: "⚙️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,test,github-actions]
published: false
---

# GitHub Actions で XCTest を実行してみる

## GitHub Actions に対応しているシミュレータ

以下の表から確認できます。

https://github.com/actions/virtual-environments/blob/main/images/macos/macos-10.15-Readme.md#installed-simulators

記事を書いている現在 iOS14.4 までなので、もしテスト実行のターゲット OS を 14.5 以降に設定していると GitHub Actions 実行時にシミュレータが存在しないため、テストが実行されずにワークフローがパスしたことになるので注意が必要です。

GitHub Actions 上では、以下の警告が表示され `Continuing with testing` と出力され、テストを続行し、ワークフローが失敗してくれないのでうまくいっているものと勘違いしやすいです。

```
warning: The iOS Simulator deployment target 'IPHONEOS_DEPLOYMENT_TARGET' is set to 14.5, but the range of supported deployment target versions is 9.0 to 14.4.99. (in target 'XCTestExampleTests' from project 'XCTestExample')
2021-08-19 02:43:16.651 xcodebuild[1100:8110]  IDETestOperationsObserverDebug: Writing diagnostic log for test session to:
/Users/runner/Library/Developer/Xcode/DerivedData/XCTestExample-ggawoeumschpccclqsuwuwavcwys/Logs/Test/Test-XCTestExample-2021.08.19_02-41-08-+0000.xcresult/Staging/1_Test/Diagnostics/XCTestExampleUITests-CC04D5F9-ADE8-4E5A-AAAE-3B8AD7342816/XCTestExampleUITests-D35706D8-F92D-4EA4-8C5B-09EFA34FD696/Session-XCTestExampleUITests-2021-08-19_024316-Aefwk0.log
2021-08-19 02:43:16.651 xcodebuild[1100:7908] [MT] IDETestOperationsObserverDebug: (667C8420-783E-4D3A-A7DC-1E9B75CF9790) Beginning test session XCTestExampleUITests-667C8420-783E-4D3A-A7DC-1E9B75CF9790 at 2021-08-19 02:43:16.651 with Xcode 12D4e on target <DVTiPhoneSimulator: 0x7fc42cf576a0> {
		SimDevice: iPhone 8 (EA2D3EF4-6484-468E-9F25-9CBC20BADC60, iOS 14.4, Shutdown)
} (14.4 (18D46))
2021-08-19 02:43:34.082 xcodebuild[1100:7908] [MT] IDETestOperationsObserverDebug: (667C8420-783E-4D3A-A7DC-1E9B75CF9790) Finished requesting crash reports. Continuing with testing.
```

## ブランチを保護する
ワークフローが成功しないとマージできないように [Branch Protection Rule](https://docs.github.com/en/github/administering-a-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule) から設定できます。

![](https://storage.googleapis.com/zenn-user-upload/2e4fcf22070386d8a3d441b0.png)

`Status checks that are required.` の部分は GitHub Actions YAML の job 名の部分を設定することでこのジョブがパスしないとマージできないようにできます。

![](https://storage.googleapis.com/zenn-user-upload/65bb6352e8aec3d4ab934a5b.png)

## ステータスをREADMEに表示する
README によく貼ってある「CI passing」のバッジは Actions からマークダウンを取得可能なので、これをコピーして README にペーストすれば表示されるようになります。

![](https://storage.googleapis.com/zenn-user-upload/af0d9820becc3b072aa17e79.png)
![](https://storage.googleapis.com/zenn-user-upload/51f57319b28c3439bd697d9e.png =400x)

# Example
これらの動きを以下のリポジトリで検証しました。
https://github.com/ykws/XCTestExample

GitHub Actions は以下のワークフローとして実行させています。
https://github.com/ykws/XCTestExample/actions/runs/1147283619/workflow

## わざとテストを失敗させる
https://github.com/ykws/XCTestExample/pull/2

ワークフローが失敗し、この状態ではマージできないことを確認できました。

![](https://storage.googleapis.com/zenn-user-upload/b96f52549942c68b32a9400c.png)

一応、この状態でも管理者権限であればマージすることは可能です。

![](https://storage.googleapis.com/zenn-user-upload/b11d59d128dc6f06756bbdb8.png =400x)

### ちゃんとテストを成功させる
https://github.com/ykws/XCTestExample/pull/3

ワークフロー実行中はマージできないようになっています。

![](https://storage.googleapis.com/zenn-user-upload/4ab26d15513e353e8d915c6e.png)

ワークフローが成功するとマージできるようになります。

![](https://storage.googleapis.com/zenn-user-upload/2fffb5f49c26227309578928.png)
