# Function Back Deployment (Summary)

* Full Proposal: [SE-0376](https://github.com/apple/swift-evolution/blob/main/proposals/0376-function-back-deployment.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: October ‘22](https://www.fline.dev/swift-evolution-monthly-october-22/#se-0376-function-back-deployment)

This proposal suggests adding a new `@backDeploy(before: ...)` attribute to Swift which none of us app or framework developers will ever need. It's only relevant for those working on system libraries shipped with the OS like `SwiftUI` or `Charts`, but this new attribute might still have a big influence on app developers, too:

Once available, Apple could start introducing new APIs not only for the shiny new operating systems (e.g. `iOS 17` & `macOS 14`) but they could also back-deploy *some* of the new APIs to older OS versions at the same time. App developers then wouldn't need to do anything specific to use back-deployed functions. Quite the opposite, they will be able to avoid availability checks and fallback code like this:

```Swift
// without back-deployment
if #available(iOS 16.0, *) {
   callSomeShinyNewFunction()
} else {
   // fallback on older systems
}

// with back-deployment
callSomeShinyNewFunction()
```

When using a function marked with `@backDeploy` the compiler will intelligently check the target SDK of the app being built against and if it's a version that natively supports the new feature, it will use the system framework, mitigating app binary size increases and each app shipping with its own copy of the function.

Now, don't get *too* excited though. While this is really cool and could mean more new APIs will be available to use for teams who need to support OS versions 1-2 years back, there are still limitations. This proposal only discusses adding the attribute for functions, subscripts, and properties without storage. Types like enums and structs or protocol conformances aren't discussed yet, they are only [mentioned](https://github.com/apple/swift-evolution/blob/main/proposals/0376-function-back-deployment.md?ref=fline.dev#future-directions) as potential future directions.

Also, there are [restrictions](https://github.com/apple/swift-evolution/blob/main/proposals/0376-function-back-deployment.md?ref=fline.dev#restrictions-on-declarations-that-may-be-back-deployed) on which functions *can* be back-deployed, such as that they need to be statically dispatchable (e.g. `final`), which excludes all functions that are marked to be compatible with Objective-C via `@objc`. This means that it will be easier for Apple to back-deploy APIs in newer Swift-only frameworks like `SwiftUI`, `Combine` or `Charts` than it will be to do so with older Objective-C-based frameworks like `UIKit` & `AppKit`. Also, we don't know how many teams within Apple will make use of this new feature and how far back they will be able to back-deploy. Only time can tell. But it's exciting to see this topic is getting some love!
