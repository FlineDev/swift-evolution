# `if` and `switch` expressions (Summary)

* Full Proposal: [SE-0380](https://github.com/apple/swift-evolution/blob/main/proposals/0380-if-switch-expressions.md)
* Summary Author: [Cihat Gündüz](https://fline.dev/about)
* Article: [Swift Evolution Monthly: December ‘22](https://www.fline.dev/swift-evolution-monthly-december-22/#se-0380-%60if%60-and-%60switch%60-expressions)

Consider this sample code showing a portion of logic for a Chess game app, read especially the comments I've added showing different implementation patterns:

```Swift
class ChessGame {
   enum Direction: CaseIterable {
      case up, right, down, left, upRight, downRight, downLeft, upLeft
   }

   enum Piece {
      case pawn, rook, knight, bishop, queen, king

      // implemented as if-else with multiple returns
      var maxFieldsCanMoveInOneTurn: Int {
         if self == .pawn || self == .king {
            return 1
         } else if self == .knight {
            return 3
         } else {
            return 7
         }
      }
   }

   func movementLogic(of piece: Piece) {
      // implemented as switch-case with multiple returns in a self-executing closure
      let movableDirectionsForWhitePlayer: [Direction] = {
         switch piece {
         case .pawn: return [.up]
         case .rook: return [.up, .right, .down, .left]
         case .bishop: return [.upRight, .downRight, .downLeft, .upLeft]
         case .knight, .queen, .king: return Direction.allCases
         }
      }()

      // implemented as switch-case with Swift's definite initialization feature
      let movableDirectionsForBlackPlayer: [Direction]
      switch piece {
      case .pawn:
         movableDirectionsForBlackPlayer = [.down]
      case .rook:
         movableDirectionsForBlackPlayer = [.up, .right, .down, .left]
      case .bishop:
         movableDirectionsForBlackPlayer = [.upRight, .downRight, .downLeft, .upLeft]
      case .knight, .queen, .king:
         movableDirectionsForBlackPlayer = Direction.allCases
      }

      // …
   }
}
```

Of course, this code could be refactored in multiple ways, but let’s stick to this code example for educational purposes. There are cases when some developers (including me) actually want this kind of in-line structure, especially if we want to return different but simple and clear values for different cases in code. Well, I have great news for you, because this proposal is doing exactly that and is simplifying the above code in many ways:

First, we no longer need to state the `return` keyword in `if` or `switch`statements if (and only if) all branches have exactly one statement and when the type of this statement is the same for all of them (independently!). Second, we no longer have to write a self-executing closure in these cases anymore, we can directly assign to variables from a `if` or `switch` statement. The latter aspect actually is what differentiates a mere `statement` from an `expression`: An expression always evaluates to a value, where an `if` or `switch` *statement* is only a structure for conditional code, a statement doesn’t (necessarily) evaluate to a value. But `if` and `switch` now will be usable as *expressions* when returning from functions/properties/closures or when assigning to variables.

With this proposal, the above code can be written as:

```Swift
class ChessGame {
   enum Direction: CaseIterable {
      case up, right, down, left, upRight, downRight, downLeft, upLeft
   }

   enum Piece {
      case pawn, rook, knight, bishop, queen, king

      // no need for any return keyword
      var maxFieldsCanMoveInOneTurn: Int {
         if self == .pawn || self == .king { 1 }
         else if self == .knight { 3 } else { 7 }
      }
   }

   func movementLogic(of piece: Piece) {
      // no return keyword, no need for self-executing closure
      let movableDirectionsForWhitePlayer: [Direction] =
         switch piece {
         case .pawn: [.up]
         case .rook: [.up, .right, .down, .left]
         case .bishop: [.upRight, .downRight, .downLeft, .upLeft]
         case .knight, .queen, .king: Direction.allCases
         }

      // no return keyword, no need for definite initialization
      let movableDirectionsForBlackPlayer: [Direction] =
         switch piece {
         case .pawn: [.down]
         case .rook: [.up, .right, .down, .left]
         case .bishop: [.upRight, .downRight, .downLeft, .upLeft]
         case .knight, .queen, .king: Direction.allCases
         }

      // …
   }
}
```

I highlighted the differences in the comments. Isn’t this much cleaner? I personally use self-executing closures from time to time, so now being able to get rid of them alongside the return keyword is really making the code read more naturally to me. Today I prefer to put each return in a `switch-case` in its own line plus an empty line before the next case for best readability, causing them to become really bloated (see [here](https://github.com/FlineDevPublic/OpenFocusTimer/blob/a33ea48b8eab6db05d9dee508982e29f839fc37a/Sources/Settings/SettingsState.swift?ref=fline.dev#L17-L42) for example). I’ll for sure consider returning in one line like this in the future!
