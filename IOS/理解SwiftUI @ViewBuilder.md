
# 理解SwiftUI @ViewBuilder


## 介绍
@ViewBuilder 是 SwiftUI 中的一个属性包装器（property wrapper），它的主要作用是允许通过更灵活的方式构建视图（View），尤其是在需要返回多个视图时。它本质上是一个函数构建器（function builder），用于将多个视图组合成一个单一的视图结构。
>ViewBuilder 会自动将多个视图包装为一个 TupleView
```swift
var body: some View {
    Text("你好")
    Text("Hello, World!")
}

```
>尽管这里有两个视图，但它们会被隐式地转换为：
```swift
var body: some View {
    ViewBuilder.buildBlock(Text("你好"), Text("Hello, World!"))
}
```
>生成的结果是一个 TupleView<(Text, Text)>，它符合 View 协议，因此不会报错

## 为什么要使用@ViewBuild?
在 SwiftUI 中，如果创建视图方法只能返回一个视图。如果在 创建视图的方法 中尝试返回多个视图，编译器会报错。以下是一个具体的例子：
```swift
func someFunction() -> some View {
    Text("你好")
    Text("Hello, World!")
}
```
>这种情况下，编译器会报错，因为 someFunction 不是一个 ViewBuilder 上下文，不能隐式地转换多个视图为单一视图。

## 推荐写法
```swift
struct testView: View {
    var body: some View {
        subView()
    }
    
    @ViewBuilder
    func subView() -> some View {
        Text("sub View1")
        Text("sub View")
    }
}
```
>在 SwiftUI 中，视图的构建方法只能返回一个视图，这是 SwiftUI 设计的一个基本原则，旨在保持视图结构的清晰和直观。这一限制通过使用容器视图来解决，容器视图可以包含多个子视图，并控制它们的布局和排列方式。