# FloatingTabView

A customizable floating tab bar component for SwiftUI, providing a modern, animated, and flexible tab navigation experience. 

## Demo

See [floating_tab.mov](./floating_tab.mov) for a demo video showcasing the FloatingTabView in action.

## Features
- Floating, capsule-shaped tab bar with smooth animations
- Customizable colors, backgrounds, and animation
- Supports hiding/showing the tab bar dynamically
- Easy integration with SwiftUI's `TabView` pattern
- Works with any enum conforming to `CaseIterable`, `Hashable`, and `FloatingTabProtocol`

## Installation
Copy the files in the `Source` directory into your project.

## Usage

1. **Define your tab enum:**
```swift
import SwiftUI

enum AppTab: String, CaseIterable, FloatingTabProtocol {
    case memories, library, albums, search

    var sysmbolImage: String {
        switch self {
        case .memories: return "memories" // Use a valid SF Symbol or your asset name
        case .library: return "photo.fill.on.rectangle.fill"
        case .albums: return "square.stack.fill"
        case .search: return "magnifyingglass"
        }
    }
}
```

2. **Use `FloatingTabView` in your root view:**
```swift
struct ContentView: View {
    @State private var activeTab: AppTab = .library
    var body: some View {
        FloatingTabView(selection: $activeTab) { tab, tabBarHeight in
            switch tab {
            case .memories:
                Text("Memories")
            case .library:
                LibraryView(tabBarHeight: tabBarHeight)
            case .albums:
                Text("Albums")
            case .search:
                Text("Search")
            }
        }
    }
}
```

3. **Hide/show the tab bar in subviews:**
```swift
struct LibraryView: View {
    @State private var hideTabBar: Bool = false
    let tabBarHeight: CGFloat
    var body: some View {
        NavigationStack {
            VStack {
                Spacer()
                Button("Hide Tab Bar") {
                    hideTabBar.toggle()
                }
            }
            .padding(.bottom, tabBarHeight + 30)
            .navigationTitle("Library")
        }
        .hideFloatingTabBar(hideTabBar)
    }
}
```

## Customization
You can customize the appearance and behavior of the tab bar by passing a `FloatTabConfig` to `FloatingTabView`:
```swift
let config = FloatTabConfig(
    activeTint: .white,
    activeBackgroundTint: .blue,
    inactiveTint: .gray,
    tabAnimation: .easeInOut(duration: 0.3),
    backgroundColor: .gray.opacity(0.1),
    insetAmount: 8,
    isTranslucent: true,
    hPadding: 20,
    bPadding: 10
)

FloatingTabView(config: config, selection: $activeTab) { ... }
```

### `FloatTabConfig` Properties
- `activeTint`: Color of the active tab icon
- `activeBackgroundTint`: Background color of the active tab
- `inactiveTint`: Color of inactive tab icons
- `tabAnimation`: Animation for tab transitions
- `backgroundColor`: Tab bar background color
- `insetAmount`: Padding inside each tab
- `isTranslucent`: Use a translucent background
- `hPadding`: Horizontal padding for the tab bar
- `bPadding`: Bottom padding for the tab bar

## Protocol
Your tab enum must conform to:
```swift
public protocol FloatingTabProtocol {
    var sysmbolImage: String { get }
}
```

## License
MIT 