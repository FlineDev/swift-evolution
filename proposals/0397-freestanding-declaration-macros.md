# Freestanding Declaration Macros (Summary)

* Full Proposal: [SE-0397](https://github.com/apple/swift-evolution/blob/main/proposals/0397-freestanding-macros.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0397-freestanding-declaration-macros)

The proposal extends Swift's macros to allow generating declarations, enabling use cases like generating data structures from templates or introducing warning/error directives as macros. It introduces freestanding declaration macros that use the `#` syntax and have type-checked arguments, producing zero or more declarations. For example, the `#warning` directive from [SE-0196](https://github.com/apple/swift-evolution/blob/main/proposals/0196-diagnostic-directives.md?ref=fline.dev) can be declared as a freestanding declaration macro like so:

```Swift
@freestanding(declaration)
macro warning(_ message: String) = #externalMacro(module: "MyMacros", type: "WarningMacro")
```
