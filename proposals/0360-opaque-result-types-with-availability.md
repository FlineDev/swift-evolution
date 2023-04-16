# Opaque result types with limited availability (Summary)

* Full Proposal: [SE-0360](https://github.com/apple/swift-evolution/blob/main/proposals/0360-opaque-result-types-with-availability.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: June ‘22](https://www.fline.dev/swift-evolution-monthly-june-22/#se-0360-opaque-result-types-with-limited-availability)

This basically widens the applicability of [SE-0244: Opaque Result Types](https://github.com/apple/swift-evolution/blob/main/proposals/0244-opaque-result-types.md?ref=fline.dev) to very simple `if #available` checks which is useful for framework authors. For example, the following example currently doesn’t compile because the `some Shape` opaque result type requires that the body returns exactly one specific type, but we have two different return types (Rectangle and Square):

```Swift
func test() -> some Shape {
	if #available(macOS 100, *) {
		return Rectangle()
	} else if #available(macOS 99, *) {
		return Square()
	}
	return self
}
```

With this proposal, the above code would compile. But don’t get too hyped, the following already doesn’t compile due to the dynamic if condition:

```Swift
func test() -> some Shape {
	if cond {
		...
	} else if #available(macOS 100, *) {  // <= not possible
		return Rectangle()
	}
	return self
}
```

So in most cases, you will still need to resort back to existential `any` ([SE-0335](https://github.com/apple/swift-evolution/blob/main/proposals/0335-existential-any.md?ref=fline.dev)) or [other means of type erasure](https://www.swiftbysundell.com/articles/different-flavors-of-type-erasure-in-swift/?ref=fline.dev) if you have multiple return types. By the way, I strongly recommend watching the WWDC22 session [Embrace Swift generics](https://developer.apple.com/videos/play/wwdc2022/110352/?ref=fline.dev) if you struggle with the new `some` and `any` keywords and when to use which. It starts with a quick introduction to protocol-oriented programming, goes into how you can replace `where T: Animal` with `some Animal`, and then explains the differences between `some` and `any` pretty well.
