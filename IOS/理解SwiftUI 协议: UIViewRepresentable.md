
# 理解SwiftUI 协议: UIViewRepresentable


## 介绍
通过遵循UIViewRepresentable协议，可以在 SwiftUI 中使用 UIKit 的视图和控件。它不仅帮助APP在迁移过程中逐步引入 SwiftUI，还能利用 UIKit 中的成熟功能和性能优化。通过实现 UIViewRepresentable，可以创建复杂的自定义视图，实现高性能的动画效果，或使用特定的 UIKit 功能，如地图和模糊效果


## 使用场景
- **桥接 UIKit 和 SwiftUI:** UIViewRepresentable 协议提供了一种方法，将 UIKit 视图转换为 SwiftUI 视图，可以在 SwiftUI 中使用 UIKit 的组件。
通过实现这个协议，可以创建一个 SwiftUI 结构体，该结构体能够生成并管理一个 UIKit 视图。
- **访问 UIKit 的功能:** 有些功能在 UIKit 中已经很成熟且功能丰富，但在 SwiftUI 中可能尚未提供或仍在发展中。通过 UIViewRepresentable，可以利用这些功能。
例如，UIVisualEffectView 用于创建模糊效果，MKMapView 用于显示地图等


## 为什么要用 UIViewRepresentable
>  #
>**利用现有代码：** 如果已经有大量的现有 UIKit 代码，使用 UIViewRepresentable 可以让逐步迁移到 SwiftUI，而不需要一次性重写所有代码
>  #####
>**访问特定功能：** 有些功能在 UIKit 中已经实现了很多年，非常稳定和强大。例如，地图、复杂的动画、自定义绘制等。通过 UIViewRepresentable，可以直接使用这些功能，而不需要等待 SwiftUI 提供相应的功能。
>  #####
>**性能优化：**  在一些特定的场景下，使用 UIKit 视图可能比纯 SwiftUI 视图更高效。例如，复杂的自定义视图或需要高性能动画的场合。
>  #


## 使用示例
**模糊效果:** 使用 UIVisualEffectView 实现毛玻璃模糊效果

```swift
struct BlurView: UIViewRepresentable {
    
    var style: UIBlurEffect.Style
    
    func makeUIView(context: Context) -> UIVisualEffectView {
        let view = UIVisualEffectView(effect: UIBlurEffect(style: style))
        
        return view
    }
    
    func updateUIView(_ uiView: UIVisualEffectView, context: Context) {
         
    }
}

// MARK: 另一个视图
var body: some View {
    ZStack {
        // MARK: 遮罩层
        BlurView(style: .systemUltraThinMaterialDark)
    }
}
```
