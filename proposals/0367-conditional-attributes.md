# Conditional compilation for attributes (Summary)

* Full Proposal: [SE-0367](https://github.com/apple/swift-evolution/blob/main/proposals/0367-conditional-attributes.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: September ‘22](https://www.fline.dev/swift-evolution-monthly-September-22/#se-0367-conditional-compilation-for-attributes)

Conditional compilation is a language feature very much needed when supporting multiple SDKs (e.g. `#if os(iOS)`) in an app or multiple Swift versions (e.g. `#if swift(>=5.6)`) in a framework. The most common use case probably is an `#if DEBUG` for code that we only want to run during development, but not in production (such as additional logging or different API targets or tokens).

But currently, if you wanted to conditionally use Swift attributes such as `@objc`or the newer `@preconcurrency` (for code that was written without Swift Concurrency in mind, see [SE-0337](https://github.com/apple/swift-evolution/blob/main/proposals/0337-support-incremental-migration-to-concurrency-checking.md?ref=fline.dev)), you'd have to **duplicate the code** for the entire thing the attribute applies to, such as for an entire type like in this example:

```Swift
#if compiler(>=5.6)@preconcurrency
   protocol P: Sendable {
      func f()
      func g()
   }
#elseprotocol P: Sendable {
      func f()
      func g()
   }
#endif
```

From Swift 5.8 onwards we will be able to shorten that code to just this:

```Swift
#if hasAttribute(preconcurrency)@preconcurrency
#endifprotocol P: Sendable {
   func f()
   func g()
}
```

Note the new `hasAttribute` conditional directive. This not only prevents duplicate code but also more clearly states *why* we were using a conditional compilation here (because of the attribute!) and *saves us the time* of having to look up which Swift version the attribute was introduced in.
