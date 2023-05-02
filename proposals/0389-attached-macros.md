# Attached Macros (Summary)

* Full Proposal: [SE-0389](https://github.com/apple/swift-evolution/blob/main/proposals/0389-attached-macros.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0389-attached-macros)

This is a continuation of [SE-0382](https://www.fline.dev/swift-evolution-monthly-jan-feb-23/#se-0382-expression-macros) which introduced "Expression Macros". While the latter focused on a more function-like structure that we could "call" from anywhere using `#`, this proposal focuses on "attaching" behavior to existing declarations like functions or types using `@`. This might remind you of [SE-0258](https://github.com/apple/swift-evolution/blob/main/proposals/0258-property-wrappers.md?ref=fline.dev) which introduced [property wrappers](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/properties/?ref=fline.dev#Property-Wrappers) that allowed attaching extra behavior to stored properties like done by SwiftUI with the likes of `@State`. And, in fact, this similarity is on purpose because, to some extent, "attached macros" are an extension of property wrappers to all kinds of declarations and are explicitly stated as "subsuming some of the[ir] behavior".

For example, you could declare a "peer macro" named `AddCompletionHandler`:

```Swift
@attached(peer, names: overloaded)
macro AddCompletionHandler(param: String = "completionHandler")
```

Then attach it to any `async` API like this (overriding the default param name):

```Swift
@AddCompletionHandler(param: "completion")
func fetchImage(url: URL) async throws -> Image { ... }
```

The macro would create an overloaded version of the same function with a non-`async` completion-handler as a parameter for you, for example:

```Swift
func fetchImage(url: URL, completion: (Image) -> Void) throws {
   Task {
      let image = try await self.fetchImage(url)
      completion(image)
   }
}
```

The implementation of a macro is a bit involved and requires an understanding of [swift-syntax](https://github.com/apple/swift-syntax?ref=fline.dev) & a new `SwiftSyntaxMacros` module. But the mere possibility to *adjust* our Swift code *using* Swift code is very powerful.

In total, there will be 5 different kinds of attached macros:

1. **Peer Macros**:Attached to declarations to provide additional declarations.
2. **Member Macros**:Attached to types to add new members (like properties).
3. **Accessor Macros**:Attached to stored properties to turn them into computed properties with full access to the local context (unlike property wrappers).
4. **Member *Attribute* Macros**:Attached to types to apply attributes to all members (similar to `@objcMembers` or `@MainActor`).
5. **Conformance Macros**:Attached to types to add new protocol conformances.

I really liked how Swift improved the developer experience by automatically synthesizing the conformance to types like `Codable` ([SE-0166](https://github.com/apple/swift-evolution/blob/main/proposals/0166-swift-archival-serialization.md?ref=fline.dev) + [SE-0295](https://github.com/apple/swift-evolution/blob/main/proposals/0295-codable-synthesis-for-enums-with-associated-values.md?ref=fline.dev)), `Hashable`, or `Equatable` (both [SE-0185](https://github.com/apple/swift-evolution/blob/main/proposals/0185-synthesize-equatable-hashable.md?ref=fline.dev)). And I'm really happy to see that we are now getting the power of implementing similarly magical APIs ourselves. 🪄
