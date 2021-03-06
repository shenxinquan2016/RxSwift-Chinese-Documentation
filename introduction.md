<img src="assets/Rx_Logo_M.png" alt="Miss Electric Eel 2016" width="36" height="36"> RxSwift: ReactiveX for Swift
======================================

[![Travis CI](https://travis-ci.org/ReactiveX/RxSwift.svg?branch=master)](https://travis-ci.org/ReactiveX/RxSwift) ![platforms](https://img.shields.io/badge/platforms-iOS%20%7C%20macOS%20%7C%20tvOS%20%7C%20watchOS%20%7C%20Linux-333333.svg) ![pod](https://img.shields.io/cocoapods/v/RxSwift.svg) [![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage) [![Swift Package Manager compatible](https://img.shields.io/badge/Swift%20Package%20Manager-compatible-brightgreen.svg)](https://github.com/apple/swift-package-manager) [![Chinese Documentation](https://img.shields.io/badge/中文文档-支持-4BC51D.svg)](https://beeth0ven.github.io/RxSwift-Chinese-Documentation/) ![QQ Group](https://img.shields.io/badge/QQ%20交流群-424180219-blue.svg)

[ReactiveX](http://reactivex.io/)（简写: Rx） 是一个可以帮助我们简化异步编程的框架。

它拓展了[观察者模式](https://zh.wikipedia.org/wiki/观察者模式)。使你能够自由组合多个异步事件，而不需要去关心线程，同步，线程安全，并发数据以及I/O阻塞。

[RxSwift](https://github.com/ReactiveX/RxSwift) 是 [Rx](https://github.com/Reactive-Extensions/Rx.NET) 的 **Swift** 版本。

它尝试将原有的一些概念移植到 iOS/macOS 平台。

你可以在这里找到跨平台文档 [ReactiveX.io](http://reactivex.io/)。

------

[RxSwift](https://github.com/ReactiveX/RxSwift) **QQ** 交流群: **424180219**

------
## 文档更新日志

#### 17年10月9日

* 给 [ReplaySubject](/content/rxswift_core/observable_and_observer/replay_subject.md) 加入演示代码
* 给 [PublishSubject](/content/rxswift_core/observable_and_observer/publish_subject.md) 加入演示代码
* 给 [distinctUntilChanged](/content/decision_tree/distinctUntilChanged.md) 操作符加入演示代码
* 给 [scan](/content/decision_tree/scan.md) 操作符加入演示代码
* 给 [startWith](/content/decision_tree/startWith.md) 操作符加入演示代码
* 给 [merge](/content/decision_tree/merge.md) 操作符加入演示代码
* **(RxSwift 4)** 使用 [Binder](/content/rxswift_core/observer/binder.md) 替换 **UIBindingObserver**，更简洁实用

  **[... 点击查看更多](CHANGELOG.md)**

---

## 示例

Github 搜索...

![](assets/GithubSearch.gif)

定义搜索结果 ...
```swift
let searchResults = searchBar.rx.text.orEmpty
    .throttle(0.3, scheduler: MainScheduler.instance)
    .distinctUntilChanged()
    .flatMapLatest { query -> Observable<[Repository]> in
        if query.isEmpty {
            return .just([])
        }
        return searchGitHub(query)
            .catchErrorJustReturn([])
    }
    .observeOn(MainScheduler.instance)
```

... 然后将结果绑定到 tableview 上

```swift
searchResults
    .bind(to: tableView.rx.items(cellIdentifier: "Cell")) {
        (index, repository: Repository, cell) in
        cell.textLabel?.text = repository.name
        cell.detailTextLabel?.text = repository.url
    }
    .disposed(by: disposeBag)
```

------

## 安装

安装 **RxSwift** 不需要任何第三方依赖。

## 必备条件

* Xcode 8.0

* Swift 3.0

### [CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html)

** **`pod --version`**: **`1.1.1` 已通过测试

```ruby
# Podfile
use_frameworks!

target 'YOUR_TARGET_NAME' do
    pod 'RxSwift',    '~> 3.0'
    pod 'RxCocoa',    '~> 3.0'
end
```

替换 `YOUR_TARGET_NAME` 然后在 `Podfile` 目录下, 终端输入：

```bash
$ pod install
```

### [Carthage](https://github.com/Carthage/Carthage)

** **`carthage version`**: **`0.18.1` 已通过测试

添加到 `Cartfile`

```
github "ReactiveX/RxSwift" ~> 3.0
```

```bash
$ carthage update
```

### [Swift Package Manager](https://github.com/apple/swift-package-manager)

**  **`swift build --version`**: **`3.0.0 (swiftpm-19)` 已通过测试

创建`Package.swift` 文件。

```swift
import PackageDescription

let package = Package(
    name: "RxTestProject",
    targets: [],
    dependencies: [
        .Package(url: "https://github.com/ReactiveX/RxSwift.git", majorVersion: 3)
    ]
)
```

```bash
$ swift build
```

### 使用 submodules 手动集成

* 添加 RxSwift 作为子模块

```bash
$ git submodule add git@github.com:ReactiveX/RxSwift.git
```

* 拖拽 `Rx.xcodeproj` 到项目中
* 前往 `Project > Targets > Build Phases > Link Binary With Libraries`, 点击 `+` 并且选中 `RxSwift-[Platform]` 和 `RxCocoa-[Platform]`
