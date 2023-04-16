# Lift all limitations on variables in result builders (Summary)

* Full Proposal: [SE-0373](https://github.com/apple/swift-evolution/blob/main/proposals/0373-vars-without-limits-in-result-builders.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: October ‘22](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0373-lift-all-limitations-on-variables-in-result-builders)

When writing code in the `body` of a view in SwiftUI, we can already use a lot of structured programming expressions such as `if` statements or `for-each`loops. But we cannot use *everything* that we can use in a function. For example, `lazy` variables or property wrappers such as `@Clamping` suggested in [this NSHipster article](https://nshipster.com/propertywrapper/?ref=fline.dev#implementing-a-value-clamping-property-wrapper) will work in a function, but not in a SwiftUI `body`.

The reason is that SwiftUI has a `[ViewBuilder](https://developer.apple.com/documentation/swiftui/viewbuilder?ref=fline.dev)` type, which is a result builder. So all  code you write inside the `body` runs in a different-than-normal Swift environment, inside the SwiftUI DSL to be exact. This DSL (= domain-specific language) code may look like normal Swift code like we write in a function (thanks to [SE-0289](https://github.com/apple/swift-evolution/blob/main/proposals/0289-result-builders.md?ref=fline.dev)), but it actually isn't. What you can do within result builders is restricted, but this new proposal aims to get rid of some of the restrictions – those on variables in particular. In the future, you will be able to write the following code and it will compile fine (without the error in the comments you currently get):

```Swift
import SwiftUI

struct ContentView: View {
   var body: some View {
      GeometryReader { proxy in
         // ⬇ 'Cannot declare local wrapped variable in builder'
         @Clamping(10...100) var width = proxy.size.width
         Text("Hello World").frame(width: width)
      }
   }
}
```

The full list of removed limitations on variables in result builders is:

- Uninitialized variables (e.g. `let x: Int` – with exceptions like SwiftUI)
- Default-initialized variables (e.g. `var comment: String?`)
- Computed variables (`get` & `set`)
- Observed variables (`willSet` & `didSet`)
- Variables with property wrappers (e.g. `@Clamping(1..10) var count = 12`)
- `lazy` variables (e.g. `lazy var max: Int = { ... }`)

I have not personally wanted to use any of these yet, but I'm glad that more restrictions were lifted for those situations where we actually *do* need them.
