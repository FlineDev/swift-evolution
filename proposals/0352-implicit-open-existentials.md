# Implicitly Opened Existentials (Summary)

* Full Proposal: [SE-0352](https://github.com/apple/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: April ‘22](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0351-regex-builder-dsl)

Before we get to the proposal, let’s first clarify the term **“Existential Type”**: Whenever you define a variable or a parameter of a protocol type, you’re creating an existential type. It’s just a shorter way of saying “any type that conforms to a given protocol, but it’s not (yet) clear which type exactly”. From the compiler’s perspective, it has to create something like a “box type” until the exact type of your variable becomes clear, this type is called “existential”.

For example, this code works in Swift 5 by creating an existential type:

```Swift
var value: Codable = ["Hello", "World"]
value = "Hello, World!"
```

The `value` variable is of type “any type that conforms to `Codable`". First, we use a `[String]`, then a plain `String` and it compiles successfully. Under the hoods, Swift 5 creates an existential box type for us implicitly and dynamically changes the type of the value stored inside it from `[String]` to `String`.

Since Swift `5.6` (thanks to [SE-0335](https://github.com/apple/swift-evolution/blob/main/proposals/0335-existential-any.md?ref=fline.dev)) we can (and should!) make this existential more explicit by adding the keyword `any` in front of `Codable`:

```Swift
var value: any Codable = ["Hello", "World"]
value = "Hello, World!"
```

Starting with Swift 6, the first example will no longer compile and we will need to add `any` whenever we’re creating an existential type, to make its creation more explicit to developers. This is important because existential types are quite limited in Swift. Some of these limitations come from their [type-erasing](https://www.swiftbysundell.com/articles/different-flavors-of-type-erasure-in-swift/?ref=fline.dev) nature and can’t be fixed, but some *can* be fixed.

Developers would run into a whole set of compilation errors and had to work around them. But we are going to see improvements in the near future, like with [SE-309](https://github.com/apple/swift-evolution/blob/main/proposals/0309-unlock-existential-types-for-all-protocols.md?ref=fline.dev) shipping with Swift `5.7`.

This proposal further improves working with existentials by smoothing out the interaction between them and generics. Calls to generic functions that would have failed with an error like “protocol ‘P’ as a type cannot conform to itself”, will now succeed. Additionally, before this, we could replace a `some View`parameter with `any View`, but not the other way around. This proposal would allow us to refactor in the other direction as well.
