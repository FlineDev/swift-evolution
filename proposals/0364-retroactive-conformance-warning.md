# Warning for Retroactive Conformances of External Types (Summary)

* Full Proposal: [SE-0364](https://github.com/apple/swift-evolution/blob/main/proposals/0364-retroactive-conformance-warning.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: July ‘22](https://www.fline.dev/swift-evolution-monthly-July-22/#se-0364-warning-for-retroactive-conformances-of-external-types)

Do you have code in your projects that extends a type from Apple or Swifts standard library to make them conform to the likes of `Equatable`, `Identifiable`, `Hashable`, or `Codable`? Did you know that it is discouraged to extend external types with custom conformances to these protocols? The reason is that in case Apple or the Swift community decides to add conformance to the type in the original framework, it's currently undefined which of the two implementations (yours or the frameworks) will be used, which can lead to unexpected behavior in your app. Note that this is only a problem for users who upgrade to a newer version of the OS but have a version of your app installed that you have not built with the new version of Xcode.

In my current project, I did a quick search on the left sidebar "Find -> Regular Expression" by pasting `extension \w+: (Equatable|Identifiable|Hashable|Codable)` and I found this extension, which I need because I use [TCA](https://github.com/pointfreeco/swift-composable-architecture?ref=fline.dev) which requires my view state types to be `Equatable`, and I need a `Binding` in some of them:

```Swift
extension Binding: Equatable where Value: Equatable {
   public static func == (left: Binding<Value>, right: Binding<Value>) -> Bool {
      left.wrappedValue == right.wrappedValue
   }
}
```

This proposal suggests showing a warning in all places where you are extending an external type with these kinds of conformances. The idea is to make developers more aware of this potential issue and convince them to re-consider conforming an external type. To silence the warning you would add the module name in front of each involved type explicitly like so (note the `SwiftUI.` and `Swift.` prefixes):

```Swift
extension SwiftUI.Binding: Swift.Equatable where Value: Swift.Equatable {
   public static func == (left: SwiftUI.Binding<Value>, right: SwiftUI.Binding<Value>) -> Bool {
      left.wrappedValue == right.wrappedValue
   }
}
```
