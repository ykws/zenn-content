---
title: "GitHub Actions で XCTest を実行する"
emoji: "⚙️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ios,github-actions]
published: false
---

# GitHub Actions で XCTest を実行してみる

## Installed Simulators
GitHub Actions に対応しているシミュレータは以下の表から確認できます。

https://github.com/actions/virtual-environments/blob/main/images/macos/macos-10.15-Readme.md#installed-simulators

iOS14.4 までなのでテスト実行のターゲット OS を 14.5 以降に設定していると GitHub Actions でシミュレータが存在しないため、テストが実行されずにパスしたことになるため注意が必要です。

```
warning: The iOS Simulator deployment target 'IPHONEOS_DEPLOYMENT_TARGET' is set to 14.5, but the range of supported deployment target versions is 9.0 to 14.4.99. (in target 'XCTestExampleTests' from project 'XCTestExample')
2021-08-19 02:43:16.651 xcodebuild[1100:8110]  IDETestOperationsObserverDebug: Writing diagnostic log for test session to:
/Users/runner/Library/Developer/Xcode/DerivedData/XCTestExample-ggawoeumschpccclqsuwuwavcwys/Logs/Test/Test-XCTestExample-2021.08.19_02-41-08-+0000.xcresult/Staging/1_Test/Diagnostics/XCTestExampleUITests-CC04D5F9-ADE8-4E5A-AAAE-3B8AD7342816/XCTestExampleUITests-D35706D8-F92D-4EA4-8C5B-09EFA34FD696/Session-XCTestExampleUITests-2021-08-19_024316-Aefwk0.log
2021-08-19 02:43:16.651 xcodebuild[1100:7908] [MT] IDETestOperationsObserverDebug: (667C8420-783E-4D3A-A7DC-1E9B75CF9790) Beginning test session XCTestExampleUITests-667C8420-783E-4D3A-A7DC-1E9B75CF9790 at 2021-08-19 02:43:16.651 with Xcode 12D4e on target <DVTiPhoneSimulator: 0x7fc42cf576a0> {
		SimDevice: iPhone 8 (EA2D3EF4-6484-468E-9F25-9CBC20BADC60, iOS 14.4, Shutdown)
} (14.4 (18D46))
2021-08-19 02:43:34.082 xcodebuild[1100:7908] [MT] IDETestOperationsObserverDebug: (667C8420-783E-4D3A-A7DC-1E9B75CF9790) Finished requesting crash reports. Continuing with testing.
```

GitHub Actions 上では、上記の警告が表示され `Countinuing with testing` とテストを続行し、失敗してくれないのでうまくいっているものと勘違いしやすいです。

## Branch Protection Rule
CI をパスしないと PR をマージできないように Branch Protection Rule を設定できます。

## CI Badge
README によく貼ってある「CI passing」のバッジは Actions からリンクを取得可能です。

## Example
以下は検証したリポジトリです。
https://github.com/ykws/XCTestExample

### Failed
わざとテストが失敗する PR を作成しました。
CI が失敗し、この状態ではマージできないことを確認できました。
https://github.com/ykws/XCTestExample/pull/2

### Succeed
テストが成功すると PR をマージできるようになります。

