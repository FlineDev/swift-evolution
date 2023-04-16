# Module Aliasing For Disambiguation (Summary)

* Full Proposal: [SE-0339](https://github.com/apple/swift-evolution/blob/main/proposals/0339-module-aliasing-for-disambiguation.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/se-0339-module-aliasing-for-disambiguation)

**Problem:**

In SwiftPM, when including two dependencies that both had a target named `Utils` for example, currently we would get a name collision error. To solve this, we would need to edit one of the dependencies (e.g. by forking) and rename the `Utils` target in one to something else, like `GameUtils`.

**Solution:**

Instead of editing the dependency, we will be able to just provide an alias name to a specific packages dependency right in our projects `Package.swift` manifest file by using the new `moduleAliases` parameter like so:

```
.target(
  name: "App",
  dependencies: [
    .product(name: "Game", moduleAliases: ["Utils": "GameUtils"], package: "swift-game"),
    .product(name: "Utils", package: "swift-draw"),
  ]
)
```

**Expected in: ~**Swift 5.7 (my guess based on [merged PR](https://github.com/apple/swift-package-manager/pull/4023?ref=fline.dev) from Feb 9th)
