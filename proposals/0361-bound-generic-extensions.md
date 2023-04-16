# Extensions on bound generic types (Summary)

* Full Proposal: [SE-0361](https://github.com/apple/swift-evolution/blob/main/proposals/0361-bound-generic-extensions.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: June ‘22](https://www.fline.dev/swift-evolution-monthly-june-22/#se-0361-extensions-on-bound-generic-types)

This is another proposal for making generics more ergonomic by allowing a more natural and expected syntax when using them. In this proposal, extensions are tackled. In short, all of the 3 following extensions statements on a generic type would in the future compile and have the same meaning:

```Swift
// only this compiles today
extension Array where Element == String { ... }

// don't compile today, but would with the proposal
extension Array<String> { ... }
extension [String] { ... }
```

Note that this will only work if all generic type arguments are provided, so if you have a type with multiple generic types and you just want to specify one in your extension, you’d have to resort back to the `where` clause.

As you can see in the above examples, even syntactical sugar variants will be supported, including optionals. So you’ll be able to write `extension String?`instead of `extension Optional where Wrapped == String`.
