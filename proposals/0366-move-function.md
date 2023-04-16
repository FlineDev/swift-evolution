# `consume` operator to end the lifetime of a variable binding (Summary)

* Full Proposal: [SE-0366](https://github.com/apple/swift-evolution/blob/main/proposals/0366-move-function.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: July ‘22](https://www.fline.dev/swift-evolution-monthly-july-22/#se-0366-move-function-%E2%80%9Cuse-after-move%E2%80%9D-diagnostic)

<aside>
ℹ️ UPDATE: The `move` keyword was renamed to `consume` after reviews.
</aside>

This proposal adds a new `move` keyword that gives developers of performance-sensitive code more fine-grained control over when a variable or parameter should be released from memory. The compiler will then ensure a "moved" variable or parameter is no longer accidentally used unless a new value is reassigned to it explicitly. Most app developers won't ever need to use this keyword, but for those interested here's a function demonstrating the use of the `move` keyword:

```Swift
func f(inout x: Int = 5) -> Int {
   print(x)  // => "5"

   let y = move x + 1
   print(x)  // error: 'x' used after being moved
   print(y)  // => "6"

   var z = move y
   print(y)  // error: 'y' used after being moved
   print(z)  // => "6"

   move z
   print(z)  // error: 'z' used after being moved

   z = 10
   print(z)  // => "10" (reassigned a new value to 'z', no error)

   x = z
   print(x)  // => "10" (reassigned a new value to 'x', no error)

   return z
}
```
