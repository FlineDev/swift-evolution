# Value and Type Parameter Packs (Summary)

* Full Proposal: [SE-0393](https://github.com/apple/swift-evolution/blob/main/proposals/0393-parameter-packs.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: March + April ‘23](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0393-value-and-type-parameter-packs)

This proposal adds two new keywords to the Swift language:

- `each`: Precedes a type (like `some`) & marks it as a "parameter pack".
- `repeat`: Precedes a type/expression & "expands" the related pack.

But what is a "parameter pack" and what does it mean to "expand" one?

In Swift when writing functions that accept a variable number of values of (potentially) differing types, the best we can do if we don't want to erase their types is to write a generic function like this:

```swift
func areEquivalent<A, B, C>(_ first: A, _ second: B, _ third: C) -> Bool

// example usage
areEquivalent("42", 42, 42.0)  // => true
```

While this function uses generics so we don't have to write many overloads to the same function with different type specifications, it is restricted to taking exactly three parameters. But what if we wanted an `areEquivalent` implementation for any number of differing types? To support up to 10 parameters, we would need to define 10 different generic methods like the above, each with a growing number of generic types. But couldn't we somehow tell Swift to do that for us?

Well, that's exactly what this proposal does by means of `each` and `repeat`:

```swift
func areEquivalent<each T>(_values: repeat each T) -> Bool
```

A "parameter pack" is what allows a function to accept an arbitrary number of arguments of any type, here: `each T`. The "expansion" of a pack is the process of unpacking its contents into individual arguments, here: `repeat each T`. The single function declaration above is "expanded" to function declarations with zero, one, two, three, and more parameters of (potentially) distinct types by Swift.

And that's not all, there's much more to this proposal – it's actually quite detailed. The above is just a short intro to parameter packs, but it doesn't even show the implementation side of it. Here's an implementation from the proposal:

```swift
func zip<each T, each U>(**firsts**: repeat each T, **seconds**: repeat each U) -> (repeat (each T, each U)) {
  return (repeat (each firsts, each seconds))
}
```

And this seems just the first step into the larger topic of variadic generics programming.
