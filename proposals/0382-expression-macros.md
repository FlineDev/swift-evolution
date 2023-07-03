# Expression Macros (Summary)

* Full Proposal: [SE-0382](https://github.com/apple/swift-evolution/blob/main/proposals/0382-expression-macros.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: Jan + Feb ‘23](https://www.fline.dev/swift-evolution-monthly-jan-feb-23/#se-0382-expression-macros)

This new feature will introduce a new kind of function-like structure called an "Expression Macro" which will come in very handy to write more expressive libraries and eliminate boilerplate in code. These new macros work very similarly to functions because they take data through arguments, do something with those data and return some new data. But the big difference is that their execution happens during compile time, which means that the data they operate on is not runtime user data, but it's the source code of your app (or library)!

Yes, you heard that right, it's like a little hook we get into the compilation process so we can eliminate having to repeat writing similar code in various places to make things in our code consistent. Because you have access to your source code in macros, they will even allow for new concepts currently impossible to write in Swift. Have you ever used `#filePath`,  `#function`, `#line`, `#selector`, `#colorLiteral`, `#imageLiteral` or other things starting with `#` in your code? Well, those are all built-in expressions that will be subsumed into macros and ship as macros in the Swift standard library. These should already give you an idea of what kind of features are possible with macros. Here are 9 more use cases for macros:

1. Domain-specific languages (DSLs) – e.g. mathematics, database queries
2. Code generation – e.g. `JSON` to `Codable` struct
3. Debugging – e.g. print value of variable before & after a function call
4. Performance optimization – replace runtime checks with compile-time checks
5. Syntax extensions – e.g. new control flow constructs or data types
6. Functional programming – e.g. simulate [curried](https://en.wikipedia.org/wiki/Currying?ref=fline.dev) functions
7. Code instrumentation – e.g. measure performance, monitor usage
8. Algorithmic complexity analysis – e.g. determine time complexity of sorting
9. Code obfuscation – e.g. for hard-coded `String`s like API keys

It's hard to tell if this proposal alone already enables all these use cases until we try this new feature out, for example, because the implementation will be sandboxed like with [SwiftPM plugins](https://github.com/apple/swift-evolution/blob/main/proposals/0303-swiftpm-extensible-build-tools.md?ref=fline.dev#security). But at least it's a big first step toward the larger [vision](https://forums.swift.org/t/a-possible-vision-for-macros-in-swift/60900/5?ref=fline.dev). While this sounds quite exciting, writing a macro has its own learning curve as you'll have to make use of the [swift-syntax](https://github.com/apple/swift-syntax?ref=fline.dev) library and a new `SwiftSyntaxMacros` module that will provide what's required to define macros.

I won't go into all details of how to implement a macro, but it's worth noting that there will be two parts to it: The declaration and the implementation. Much like with functions, where `func description(count: Int) -> String` is the declaration and the code within `{ ... }` is the implementation. But with macros, those two are not necessarily within the same file and can/must be written separately:

```Swift
import SwiftSyntax
import SwiftSyntaxBuilder
import _SwiftSyntaxMacros

// Implementation:
public struct StringifyMacro: ExpressionMacro {
  public static func expansion(
    of node: some FreestandingMacroExpansionSyntax,
    in context: some MacroExpansionContext
  ) -> ExprSyntax {
    guard let argument = node.argumentList.first?.expression else {
      fatalError("compiler bug: the macro does not have any arguments")
    }

    return "(\(argument), \(literal: argument.description))"
  }
}

// Declaration:
@freestanding(expression)
macro stringify<T>(_: T) -> (T, String) =
  #externalMacro(module: "ExampleMacros", type: "StringifyMacro")

// Usage:
let (a, b): (Double, String) = #stringify(1 + 2)
```

[Doug Gregor](https://twitter.com/dgregor79?ref=fline.dev) from the Language Workgroup is maintaining [a GitHub repo](https://github.com/DougGregor/swift-macro-examples?ref=fline.dev) with many more sample implementations of macros if you're interested.

I'm really looking forward to what new expression macros Apple will introduce during WWDC this year, and I'm also sure the community will come up with some creative uses to simplify app coding life, make code safer or streamline the API of their libraries.
