# New access modifier: `package` (Summary)

* Full Proposal: [SE-0386](https://github.com/apple/swift-evolution/blob/main/proposals/0386-package-access-modifier.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: May ‘23](https://www.fline.dev/swift-evolution-monthly-may-23/#se-0386-new-access-modifier-package)

Currently, Swift library authors that like to split their code into separate modules have to use the `public` access modifier to make any APIs available from one (helper) module to another. This makes these APIs available to all users of the package, even if the API is not intended for public use. Library authors have to stick to a single module design to actually keep helper functions internal to the package.

Not anymore! The new access modifier `package` can be used in the future to mark any API as "internal to the package", making it available to other modules within the same package, but not accessible to users of the package. 🎉
