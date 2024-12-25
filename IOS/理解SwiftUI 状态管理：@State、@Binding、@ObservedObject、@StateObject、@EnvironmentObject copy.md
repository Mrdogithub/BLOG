
# 理解SwiftUI 状态管理：@State、@Binding、@ObservedObject、@StateObject、@EnvironmentObject


## 介绍
在 SwiftUI 中，状态管理是构建动态和响应式用户界面的核心。SwiftUI 提供了一系列的状态管理工具，允许开发者高效地管理视图的状态，并确保界面能够自动响应状态的变化。以下是 SwiftUI 中常见的几种状态管理方式及其使用场景：

### @State: 管理视图内状态 
@State 是一个属性包装器，用于管理视图的本地状态。它通常用于存储简单的、临时性的状态，这些状态只在该视图内使用。

#### 使用场景
- **临时状态:** 适用于视图内部使用的简单状态，例如开关的状态（Bool）、输入框的内容（String）等。
- **不需要持久化的状态:** 状态不需要在视图之间共享或持久化。
```swift
struct ContentView: View {
    @State private var isOn: Bool = false
    var body: some View {
        Toggle("Switch", isOn: $isOn)
            .padding()
    }
}
```

### @Binding: 管理视图之间双向绑定 
@Binding允许子视图绑定到父视图的状态，从而实现双向数据绑定。子视图可以读取和修改父视图的状态。
#### 使用场景：
- **父子视图之间的状态共享:** 当子视图需要修改父视图的状态时使用。
- **状态共享但不持久化:** 适用于需要共享状态的场景，但状态不需要持久化。
```swift
    struct ParentView: View {
        @State private var isOn: Bool = false
        
        var body: some View {
            VStack {
                Text("Switch is \(isOn ? "On" : "Off")")
                ChildView(isOn: $isOn)
            }
        }
    }

    struct ChildView: View {
        @Binding var isOn: Bool
        
        var body: some View {
            Toggle("Toggle Switch", isOn: $isOn)
        }
    }
```
### @ObservedObject: 多个视图之间共享状态（一方发布多方订阅）
@ObservedObject 用于管理外部对象的状态（观察和响应来自外部的 ObservableObject 的变化），通常是一个遵循 ObservableObject 协议的类。当对象的状态发生变化时，订阅视图会自动更新。

#### 使用场景：
- **共享状态:** 适用于需要在多个视图之间共享的状态。
- **复杂状态管理:** 当状态管理比较复杂，难以通过简单的 @State 管理时使用。
- **依赖外部对象:** 当状态需要由外部对象管理时使用，即发布订阅机制，一方发布，多方订阅，状态由发布方管理。
  
``` swift
class UserSettings: ObservableObject {
    @Published var username: String = "Guest" // 由数据库更数据后，订阅 username 的视图都会更新视图
}

struct ContentView: View {
    @ObservedObject var settings = UserSettings()
    
    var body: some View {
        VStack {
            Text("Username: \(settings.username)")
            TextField("Enter username", text: $settings.username)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
        }
    }
}
```

### @StateObject: 视图内部创建和管理 ObservableObject 的生命周期。
在 SwiftUI 中，@StateObject 和 @ObservedObject 都是用于管理对象生命周期和观察对象变化的属性包装器，但它们在对象的生命周期管理和初始化方式上有一些关键的区别。

- **生命周期管理：**@StateObject 用于视图内部创建和管理 ObservableObject 的生命周期，包括在视图初始化时创建对象，并在视图销毁时自动释放对象。

- **初始化：**@StateObject 在声明时需要进行初始化，因为它是负责创建对象的。

#### 使用场景：
- 通常用于视图内部定义的数据模型，或者当视图需要拥有并管理对象的生命周期时。
```swift

// MARK: SharedModel 是一个 ObservableObject，包含一个 @Published 属性 message。
// MODEL 层
class SharedModel: ObservableObject {
    @Published var message = "Hello, World!"
}

// MARK: DetailView 使用 @ObservedObject 来观察 SharedModel 的变化，但不负责其生命周期。
// VIEW MODEL 层
struct DetailView: View {
    @ObservedObject var model: SharedModel
    
    var body: some View {
        Text(model.message)
    }
}

// MARK: ContentView 使用 @StateObject 来创建和管理 SharedModel 的实例。
// VIEW 层
struct ContentView: View {
    @StateObject private var sharedModel = SharedModel()
    
    var body: some View {
        NavigationView {
            VStack {
                // MARK: 通过将 sharedModel 从 ContentView 传递给 DetailView，
                // 两个视图可以共享同一个模型实例，并且当模型发生变化时，两个视图都会更新。
                DetailView(model: sharedModel)
                Button("Update Message") {
                    sharedModel.message = "Message Updated!"
                }
            }
        }
    }
}
```

### @EnvironmentObject: 在多个视图之间共享的全局状态
@EnvironmentObject 用于在整个应用环境中共享状态。它允许状态在多个视图之间共享，而无需显式传递状态。
使用场景：
- **全局状态共享**: 适用于需要在多个视图之间共享的全局状态。
- **避免状态传递**: 当状态需要在多个层次的视图之间共享时使用。
```swift
class UserSettings: ObservableObject {
    @Published var username: String = "Guest"
}

struct ContentView: View {
    @EnvironmentObject var settings: UserSettings
    
    var body: some View {
        VStack {
            Text("Username: \(settings.username)")
            TextField("Enter username", text: $settings.username)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
        }
    }
}

@main
struct MyApp: App {
    @StateObject private var settings = UserSettings()
    
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(settings)
        }
    }
}
```
![示例图片](https://raw.githubusercontent.com/Mrdogithub/BLOG/refs/heads/main/IOS/pic/2024122501.png)