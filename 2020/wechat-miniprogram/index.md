# 知识点整理

## 2020-02-26

## 1.分包预加载

- 整体不大于 8M，单包不大于 2M，插件大小计算在主包内

## 2020-03-25

## 1. 从 getSystemInfoSync 获取 windowHeight 值，不能准确得到对应高度的坑

- 知识点：

  - getSystemInfoSync 的 screenHeight：屏幕的高度
  - getSystemInfoSync 的 windowHeight：屏幕高度-底部导航高度(tabbar)

- bug 情况：

  - 随机出现，几率极小，在 Android 设备，在 scroll-view 视图中，出现 scroll-view 高度计算错误，高度变小的情况

- bug 分析：

  - 在计算 scroll-view 高度时，使用的公式为，

  `scrollViewHeight=windowHeight-statusHeight-44(顶部标题栏高度)-55(底部按钮/tabbar高度)`

  - 被减数为 windowHeight，减数为（statusHeight-44-55）是个常量，也就是说造成该 bug 的极有可能是 windowHeight 变化造成的
    - 继续了解到，在自定义标题栏的情况下（navigationStyle: 'custom'），屏幕高度 screenHeight 和窗口高度 windowHeight 是相等的，两者都可以使用。也就是说 windowHeight 的值与 tabbar 存在与否相关
    - 同时在网上搜索到 windowHeight 的值可能不能准确拿到对应的高度，总是能拿到屏幕高度，并且在 Android 设备上易出现。

- bug 原因推测：

  - windowHeight 获取到的高度不准确，原本应该是 screenHeight，但是获取到的高度为 screenHeight-tabbarHeight，导致滑动高度变小出现了这个 bug

- 解决方案（如何得到准确 scrollViewHeight）：

  - 方案一：使用 screenHeight 作为被减数而不是 windowHeight
  - 方案二：`wx.getSystemInfoSync`是在页面初始化时提前计算，所以对于 windowHeight 这种需要进行功能判断的属性，应该使用异步接口，实时获取，所以建议采用异步接口`wx.getSystemInfo`
  - 方案三：windowHeight 的值与 tabbar 存在与否有关，为了保证在 tabbar 显示后再进行取值，建议在生命周期的 onReady 中调用 wx.getSystemInfo

  ```javascript
  onReady() {
    wx.getSystemInfo({
        success({windowHeight}) {
        // todo
        }
    });
  }
  ```
