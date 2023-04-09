# Lightweight same-type requirements for primary associated types (Summary)

* Proposal: [SE-0346](https://github.com/apple/swift-evolution/blob/main/proposals/0346-light-weight-same-type-syntax.md)
* Summary Author: [Cihat Gündüz](https://github.com/Jeehut)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0346-lightweight-same-type-requirements-for-primary-associated-types)

**Problem:**

Have you ever written a function for a concrete type, like for `Array`:

```Swift
func concatenate(_ lhs: Array<String>, _ rhs: Array<String>) -> Array<String> {
  ...
}
```

And then later you wanted to generalize it over all types conforming to `Sequence` to also make it work with `Set`, `Dictionary` and more like this:

```Swift
func concatenate<S : Sequence>(_ lhs: S, _ rhs: S) -> S where S.Element == String {
  ...
}
```

You find it weird that we specify the `where` clause at the end? And you find it cumbersome to lookup the name of the `associatedtype` (here `Element`) within the implementation of the `Sequence` protocol? These and other annoyances around generics exist at the moment, see also [this forum thread](https://forums.swift.org/t/improving-the-ui-of-generics/22814/1?ref=fline.dev).

**Solution:**

Consistent to how we write generic type requirements for concrete types like `Array<String>`, we would be able to specify the associated type right in-place:

```Swift
func concatenate<S : Sequence<String>>(_ lhs: S, _ rhs: S) -> S {
  ...
}
```

We could emit the `where` in more places, all these pairs would be equivalent:

```Swift
// Extensions
extension Collection where Element == String { ... }
extension Collection<String> { ... }

// Inheritance
protocol TextBuffer : Collection where Element == String { ... }
protocol TextBuffer : Collection<String> { ... }

// Nested Opaque Parameters
func sort<C : Collection, E : Equatable>(elements: inout C) {} where C.Element == E
func sort(elements: inout some Collection<some Equatable>) {}
```

**Expected in:** Swift 5.7 or later (partially implemented, feature flag on `main`)