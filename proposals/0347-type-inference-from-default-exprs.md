# Type inference from default expressions (Summary)

* Full Proposal: [SE-0347](https://github.com/apple/swift-evolution/blob/main/proposals/0347-type-inference-from-default-exprs.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0347-type-inference-from-default-expressions)

Currently, providing a default value for generic parameters produces an error:

```Swift
// => Error: Default argument value of type '[Int]' cannot be converted to tvpe 'C'
func compute<C: Collection>(values: C = [0, 1, 2]) { 
	// ...
}
```

Starting with ([probably](https://github.com/apple/swift/pull/41436?ref=fline.dev)) Swift `5.7`, the above code will compile **success**fully! Here’s another example of what will be possible with the usage side included:

```Swift
struct Box<F: Flags> {
    init(flags: F = DefaultFlags()) {
    	// ...
    }
}

Box() // -> Box<DefaultFlags>
Box(flags: CustomFlags()) // -> Box<CustomFlags>
```
