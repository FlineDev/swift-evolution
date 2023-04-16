# Distributed Actor Runtime (Summary)

* Full Proposal: [SE-0344](https://github.com/apple/swift-evolution/blob/main/proposals/0344-distributed-actor-runtime.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: First Issue](https://www.fline.dev/swift-evolution-monthly-first-issue/#se-0344-distributed-actor-runtime)

**Problem:**

This is a large one — so large it even has a Table of Contents! — and I don’t fully understand it yet myself. But together with [SE-0336 Distributed Actor Isolation](https://github.com/apple/swift-evolution/blob/main/proposals/0336-distributed-actor-isolation.md?ref=fline.dev), which was accepted back in January, it seems this is one of the more amazing and unique features Swift will provide compared to other modern languages.

The problem, as far as I understand, is that the newly introduced `actor` type provides safety only on a *local*environment level, as in “safety for access between different threads on the same process of one machine”. (If you’re new to `actor`s in general, Paul Hudson has a great write-up [on that, too](https://www.hackingwithswift.com/quick-start/concurrency/what-is-an-actor-and-why-does-swift-have-them?ref=fline.dev).) An `actor` type does not provide safety in accessing data between different processes or even different machines though. This makes writing correct code accessing the same data across machines hard, for example in client-server situations or in live peer-to-peer communication.

**Solution:**

Besides the new `distributed actor` type and `DistributedActor` protocol accepted in [SE-0336](https://github.com/apple/swift-evolution/blob/main/proposals/0336-distributed-actor-isolation.md?ref=fline.dev), a new `DistributedActorSystem` protocol is introduced here which basically defines details about how communication between different environments happens. If I understand it correctly, different implementations for different purposes can be provided, such as one for cross-process communication, one for clustering and another for client-server communication. In the end, we would be able to define a function as being `distributed` much like we can now define functions to be `async` like so:

```Swift
distributed actor Game {
  var state: GameState = ...

  // players can be located on different nodes
  var players: Set<Player> = []

  distributed func playerJoined(_ player: Player) {
    others.append(player)
    if others.count >= 2 { // we need 2 other players to start a game
      Task { try await self.start() }
    }
  }

  func start() async throws { ... }
}
```

Then all calls into such functions need to be `await`ed for like here:

```Swift
await game.playerJoined(newPlayer)
```

So the overall goal of this is to make distributed environment access to data (nearly) as simple as thread-safe access to data has become with `actor`s. Does this hold the potential to even eliminate all our client-server networking code in our apps, provided that the server is implemented in Swift, too? I have no idea. And from what I hear, that’s not the main goal here. But it sounds like a very interesting area to keep an eye on in the coming years ahead for sure!

**Expected in: **Swift 5.8 or later (partially implemented, feature flag on `main`)
