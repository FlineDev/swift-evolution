# Deprecate @UIApplicationMain and @NSApplicationMain (Summary)

* Full Proposal: [SE-0383](https://github.com/apple/swift-evolution/blob/main/proposals/0383-deprecate-uiapplicationmain-and-nsapplicationmain.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: Jan + Feb ‘23](https://www.fline.dev/swift-evolution-monthly-jan-feb-23/#se-0383-deprecate-uiapplicationmain-and-nsapplicationmain)

The 'Introduction' section of this proposal summarizes this pretty well:

> @UIApplicationMain and @NSApplicationMain used to be the standard way for iOS and macOS apps respectively to declare a synthesized platform-specific entrypoint for an app. These functions have since been obsoleted by SE-0281's introduction of the @main attribute, and they now represent a confusing bit of duplication in the language. This proposal seeks to deprecate these alternative entrypoint attributes in favor of @main in pre-Swift 6, and it makes their use in Swift 6 a hard error.
> 

There's not much more to say about it. But you could use this as a reminder to switch from `@UIApplicationMain` to `@main` today to prevent the warning and be Swift-6 conformant. Or wait for the fix-it to appear in a future Xcode release.
