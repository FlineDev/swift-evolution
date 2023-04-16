# Opening existential arguments to optional parameters (Summary)

* Full Proposal: [SE-0375](https://github.com/apple/swift-evolution/blob/main/proposals/0375-opening-existential-optional.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: October ‘22](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0375-opening-existential-arguments-to-optional-parameters)

First, let's remember that an 'Existential' is something like an auto-generated "placeholder box type" when working with variables of a Protocol type – see also my more detailed explanation in the April issue [here](https://www.fline.dev/swift-evolution-monthly-april-22/#se-0352-implicitly-opened-existentials). Now consider this code:

```Swift
func persist<T: Codable>(_ codable: T?, key: String) {
   if let codable {
      storageDict[key] = try! JSONEncoder().encode(codable)
   } else {
      storageDict.removeValue(forKey: key)
   }
}

func valueChanged(value: any Codable, key: String) {
   // ⬇ 'Type 'any Codable' cannot conform to 'Codable''
   persist(value, key: key)
}
```

Currently, we get an error when trying to pass the existential `any Codable` to the `persist` method which accepts a specific type `T` that conforms to `Codable`. Thanks to this new proposal, this will compile in the future as `value`here is a non-optional and therefore the underlying type can be determined and `T` is clear.
