
# 理解 SwiftUI Picker 选择器：用法和样式技巧


## 介绍
选择器是由 SwiftUI 提供的 UI 元素，使我们的用户在使用 iOS 应用时能够从多个选项中进行选择。在本文中，我们将探讨这些选择器，查看它们的各种类型，并探索它们是如何声明、配置和样式的。与 SwiftUI 按钮一样，选择器是 iOS 应用中最常用的 UI 元素之一。

### SwiftUI 选择器的类型
SwiftUI 提供了多种类型的选择器供我们使用：普通的、更加可配置的**Picker**选择器，以及专门的 **DatePicker** 和 **ColorPicker**。现在我们来逐一了解这些选择器及其子变体，看看它们的外观及其使用方式，以及如何更改选择器的样式。

- **普通选择器视图（Regular Picker View）**：这是一种基本的选择接口，用于从选项列表中进行选择。
- **日期选择器视图（DatePicker View）**：专门用于选择日期和时间。
- **颜色选择器视图（ColorPicker View）**：使用户能够从视觉调色板中选择颜色。

## SwiftUI Picker 选择器视图
SwiftUI Picker 选择器视图是可定制的，我们可以通过自定义选项来填充它，这些选项通常是一个集合或数组。以下是一个普通选择器视图的简单声明示例：


```swift
import SwiftUI
struct ContentView: View {
    @State private var selected: String = ""
    
    private let selectionOptions = [ // 数据集合
        "my first option",
        "my second option",
        "my third option"
    ]
    
    var body: some View {
        Picker("Picker Name", // 选择器名称
                selection: $selected, // 绑定数据
                content: {
                    ForEach(selectionOptions, id: \.self) {
                        Text($0)
                    }
                }
        )
    }
}
```

### 选择器声明的三个主要元素是：

- **标题：**可以是一个字符串或一个标签。
- **绑定变量：**当选择器的值发生变化时，存储该值的地方。
- **值列表：**这是用户将看到的所有选项的集合。

在这个例子中，选中的值,将通过绑定值存储到 selected 状态变量中。在示例中，Picker 方法有一个名为 content 的参数，它接收一个 Swift 闭包，即可以传递的代码块。


### SwiftUI 选择器样式

我们可以通过添加 .pickerStyle() 修饰符来更改默认的选择器样式。

**菜单选择器（Menu Picker）**
菜单选择器提供了一个上下文菜单，用户可以从其中选择一个选项。声明一个菜单选择器只需要在 Picker 后面添加 **.pickerStyle(.menu)**。

```swift
var body: some View {
    List {
        Picker("选择器名称",
               selection: $selected) {
            ForEach(selectionOptions, id: \.self) {
                Text($0)
            }
        }.pickerStyle(.menu)
    }
}
```

![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121701.webp)




### 行内选择器（Inline Picker）
行内选择器与菜单选择器非常相似，主要区别在于它默认显示所有可用选项，而不是在点击选中的项目时展开的选项列表。要声明一个行内选择器，只需在 **Picker** 后面添加 **.pickerStyle(.inline)**。

```swift
var body: some View {
    NavigationStack {
        List {
            Picker("选择器名称",
                   selection: $selected) {
                ForEach(selectionOptions, id: \.self) {
                    Text($0)
                }
            }.pickerStyle(.inline)
        }
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121702.webp)


### 滚轮选择器（Wheel Picker）
滚轮选择器是经典的滚轮界面，用户可以通过旋转来选择某个选项。当选项列表很长时，滚轮选择器会比行内选择器更紧凑，是显而易见的选择。要声明一个滚轮选择器，只需在 **Picker** 后面添加 **.pickerStyle(.wheel)**。

```swift
var body: some View {
    NavigationStack {
        List {
            Picker("选择器名称",
                   selection: $selected) {
                ForEach(selectionOptions, id: \.self) {
                    Text($0)
                }
            }.pickerStyle(.wheel)
        }
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121703.webp)


### 分段选择器（Segmented Picker）
分段选择器以一行的形式显示，通常在 iOS 应用中称为分段控件。它们非常适合在基于选择选项细分视图时使用，当用户选择其中一个选项时，可以显示不同的元素。声明一个分段选择器只需要在 **Picker** 后面添加 **.pickerStyle(.segmented)**。
```swift
var body: some View {
    NavigationStack {
        List {
            Picker("选择器名称",
                   selection: $selected) {
                ForEach(selectionOptions, id: \.self) {
                    Text($0)
                }
            }.pickerStyle(.segmented)
        }
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121704.webp)

### 导航链接选择器（Navigation Link Picker）

导航链接选择器由一行组成，当点击时，会将用户带到一个新的屏幕，显示所有选项。一旦选择了某个选项，它会自动返回到上一个屏幕，同时保持选择状态。要声明一个导航链接选择器，只需在 **Picker** 后面添加 **.pickerStyle(.navigationLink)**。

与其他选择器相比，导航链接选择器需要包含在导航栈中，而其他选择器只需在列表或表单中即可。

```swift
var body: some View {
    NavigationStack {
        List {
            Picker("选择器名称",
                   selection: $selected) {
                ForEach(selectionOptions, id: \.self) {
                    Text($0)
                }
            }.pickerStyle(.navigationLink)
        }
    }
}
```

![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121705.webp)

## SwiftUI 日期选择器
日期选择器用于用户指定日期的控件。与其他选择器一样，日期选择器也提供了多种样式，以便在不同情况下使用。接下来，我们来看看这些样式。

### 滚轮日期选择器（Wheel Date Picker）
滚轮日期选择器是最经典和最常见的日期选择方式。滚轮日期选择器的用法非常直观，可以通过以下日期选择器样式来配置它：**.datePickerStyle(WheelDatePickerStyle())**。
```swift
import SwiftUI
struct ContentView: View {
    @State private var selectedDate = Date()
    
    var body: some View {
        VStack {
            Text("选择日期")
            
            DatePicker("选择一个日期",
                       selection: $selectedDate,
                       displayedComponents: .date)
            .datePickerStyle(WheelDatePickerStyle())
        }
        .padding()
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121706.webp)

### 内联日期选择器（Inline Date Picker）
内联日期选择器在界面中以行内样式显示，类似于表单中的输入框。这种样式适合在界面空间有限的情况下使用。
```swift
struct ContentView: View {
    @State private var selectedDate = Date()
    
    var body: some View {
        VStack {
            Text("选择日期")
            
            DatePicker("选择一个日期",
                       selection: $selectedDate,
                       displayedComponents: .date)
            .datePickerStyle(InlineDatePickerStyle())
        }
        .padding()
    }
}
```



### 图形日期选择器（Graphical Date Picker）
图形日期选择器提供了类似于日历视图的界面，用户可以通过点击日期来选择。这种样式适合需要用户在日期范围内进行选择的场景。
```swift
struct ContentView: View {
    @State private var selectedDate = Date()
    
    var body: some View {
        VStack {
            Text("选择日期")
            
            DatePicker("选择一个日期",
                       selection: $selectedDate,
                       displayedComponents: .date)
            .datePickerStyle(GraphicalDatePickerStyle())
        }
        .padding()
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121707.webp)

### 紧凑日期选择器（Compact Date Picker）
紧凑日期选择器以紧凑的形式显示，适合在界面空间非常有限的情况下使用。它通常显示为一个按钮，点击后会弹出一个日期选择器。
```swift
struct ContentView: View {
    @State private var selectedDate = Date()
    
    var body: some View {
        VStack {
            Text("选择日期")
            
            DatePicker("选择一个日期",
                       selection: $selectedDate,
                       displayedComponents: .date)
            .datePickerStyle(CompactDatePickerStyle())
        }
        .padding()
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121708.webp)

### SwiftUI 颜色选择器
颜色选择器是最近几个 iOS 版本中的新增功能，它大大简化了用户从调色板中选择颜色的过程。

在某种程度上，颜色选择器比前面介绍的所有其他选择器更简单，因为它甚至没有样式属性。声明和使用颜色选择器非常简单，可以为我们提供无缝的用户体验，轻松实现在我们的应用中。以下是如何在 SwiftUI 中声明和使用颜色选择器：

## 声明颜色选择器
颜色选择器可以通过简单的 ColorPicker 视图来声明。你需要一个 @State 变量来存储用户选择的颜色。以下是基本的使用示例：
```swift
struct ContentView: View {
    @State private var bgColor = Color(
        .sRGB,
        red: 0.98,
        green: 0.9,
        blue: 0.2)
    var body: some View {
        List {
            ColorPicker(
                "Choosen Color",
                selection: $bgColor)
            }
        }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024121709.webp)