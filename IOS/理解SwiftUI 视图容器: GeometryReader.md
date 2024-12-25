
# 理解SwiftUI 视图容器: GeometryReader


## 介绍
GeometryReader 是 SwiftUI 中的一个视图容器，用于获取其父视图的尺寸信息，并将这些信息传递给其子视图。通过 GeometryReader，子视图可以根据父视图的尺寸动态调整其布局和行为。

## 为什么要用 GeometryReader
>  #
>**响应式布局：** GeometryReader 使得视图能够根据父视图的尺寸动态调整布局，从而实现响应式设计。
>  #####
>**灵活性：** 通过 GeometryReader，你可以创建灵活的布局，能够适应不同的屏幕尺寸和设备方向。
>  #####
>**精确控制：**  能够精确控制子视图的大小和位置，尤其是在需要根据父视图的尺寸进行调整时。
>  #####
>**UI 自适应:** GeometryReader 使得 UI 能够自适应不同的环境，例如不同的设备、屏幕方向、窗口大小等。
>  #


## 注意事项
>#
>**性能影响：** GeometryReader 会占用父视图的空间，并且在父视图的尺寸变化时会重新计算布局，可能会影响性能。因此，应谨慎使用，避免在频繁变化的视图中使用。
>###
>**递归问题：** 在使用 GeometryReader 时，需要注意避免递归调用（例如，子视图的尺寸依赖于父视图的尺寸，而父视图的尺寸又依赖于子视图的尺寸），这可能会导致布局循环
>#
## 使用场景
- **动态调整视图大小** 当需要根据父视图的尺寸动态调整子视图的大小时，可以使用 GeometryReader。

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        GeometryReader { geometry in
            Rectangle()
                .fill(Color.blue)
                // MARK: GeometryReader 获取父视图的尺寸（geometry.size）。
                // MARK: 使用 geometry.size.width 和 geometry.size.height 动态调整矩形的大小。
                // MARK: 结果：矩形的大小是父视图尺寸的一半。
                .frame(width: geometry.size.width * 0.5, height: geometry.size.height * 0.5)
        }
    }
}
```
- **中心对齐** 当需要在父视图的中心位置放置一个子视图时，可以使用 GeometryReader。
``` swift
  import SwiftUI
    struct ContentView: View {
        var body: some View {
            // MARK: GeometryReader 获取父视图的尺寸。
            GeometryReader { geometry in
                Circle()
                    .fill(Color.red)
                    .frame(width: 100, height: 100)
                    // MARK: 使用 geometry.size.width / 2 和 geometry.size.height / 2 计算中心位置。
                    .position(x: geometry.size.width / 2, y: geometry.size.height / 2)
            }
            // MARK:结果：圆形被放置在父视图的中心位置。
        }
    }
```

- **复杂布局** 当需要创建复杂的布局，例如根据屏幕尺寸动态调整多个子视图的位置和大小，可以使用 GeometryReader。
``` swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        // MARK: GeometryReader 获取父视图的尺寸。
        GeometryReader { geometry in
            VStack {
                Rectangle()
                    .fill(Color.green)
                    // MARK: 使用 geometry.size.width 和 geometry.size.height 动态调整多个矩形的大小和位置。
                    .frame(width: geometry.size.width, height: geometry.size.height * 0.3)
                
                HStack {
                    Rectangle()
                        .fill(Color.yellow)
                        .frame(width: geometry.size.width * 0.5, height: geometry.size.height * 0.4)
                    
                    Rectangle()
                        .fill(Color.orange)
                        .frame(width: geometry.size.width * 0.5, height: geometry.size.height * 0.4)
                }
                .frame(height: geometry.size.height * 0.4)
                
                Rectangle()
                    .fill(Color.purple)
                    .frame(width: geometry.size.width, height: geometry.size.height * 0.3)
            }
        }
    }
}

```

## 如何使用
withAnimation 的使用非常简单，只需要将状态变化的代码包裹在 withAnimation 的闭包中即可（withAnimation {...状态变化的代码}）。通过传递一个可选的动画类型（如 .easeInOut、.spring() 等）来指定动画的效果。

### 默认动画
```swift
    withAnimation {
        // 状态变化的代码
    }
```

### 自定义动画:
#### 使用场景：
```swift
    withAnimation(.easeInOut(duration: 1.0)) {
        // 状态变化的代码
    }
```
