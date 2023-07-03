# Tuple of value pack expansion (Summary)

* Full Proposal: [SE-0399](https://github.com/apple/swift-evolution/blob/main/proposals/0399-tuple-of-value-pack-expansion.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0399-tuple-of-value-pack-expansion)

Also building upon [SE-0393](https://www.fline.dev/swift-evolution-monthly-mar-apr-23/#se-0393-value-and-type-parameter-packs), this one enables referencing a *tuple* value that contains a value pack inside a pack repetition pattern. Currently, there is no way to reference individual elements of a value pack within a tuple or pass them as arguments to a function. This proposal fills that gap by allowing pack repetition patterns to operate on tuple values containing value packs, providing a method to access individual value pack elements within a tuple. For example:

```Swift
func tuplify<each T>(_value: repeat each T) -> (repeat each T) {
  return (repeat each value)
}

func example<each T>(_value: repeat each T) {
  let abstractTuple = tuplify(repeat each value)
  repeat print(each abstractTuple) // okay as of this proposal
}
```
