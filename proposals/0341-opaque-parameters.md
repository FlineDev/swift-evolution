# Opaque Parameter Declarations (Summary)

* Full Proposal: [SE-0341](https://github.com/apple/swift-evolution/blob/main/proposals/0341-opaque-parameters.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0341-opaque-parameter-declarations)

**Problem:**

Today, we can just return `some View` from functions instead of having to specify a generic type `<T: View>`. This kind of return type with the `some`keyword is called an “opaque type” (see [SE-0244](https://github.com/apple/swift-evolution/blob/main/proposals/0244-opaque-result-types.md?ref=fline.dev)). It saves us boilerplate code. If you’re like me, you might have tried to specify `some View` in parameters of functions as well, when trying to extract some SwiftUI code from a `body` that got too long. But it doesn’t currently compile…

**Solution:**

In the future it will compile! For example, instead of writing this:

```Swift
func horizontal<V1: View, V2: View>(_ v1: V1, _ v2: V2) -> some View {
  HStack {
    v1
    v2
  }
}
```

You’ll be able to write this (note that no generic types are needed):

```Swift
func horizontal(_ v1: some View, _ v2: some View) -> some View {
  HStack {
    v1
    v2
  }
}

```

**Expected in:** Swift 5.7 (confirmed in ‘Status’ field)
