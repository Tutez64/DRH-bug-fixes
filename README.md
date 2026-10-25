# Dungeon Rampage fixes

Bug fixes found while working on [Dungeon Rampage Haxe](https://github.com/Tutez64/Dungeon-Rampage-Haxe) (DRH), a Haxe port of Dungeon Rampage. Each one targets code from the official game, gathered here so the Dungeon Rampage team can apply them to the AS3 source.

## How to read a fix

Each folder in `fixes/` holds:

- `README.md`: symptom, cause, fix, where the code is in the AS3 source, and whether the bug was seen in the official game.
- `fix.diff`: the change as committed and tested in DRH.

The diffs are in Haxe, against DRH's conversion of build 25038329. The logic maps one to one to AS3; only the syntax differs:

- Haxe package paths are lowercase (`src/town/MapTownSubState.hx` is `Town/MapTownSubState.as`). Each README gives the AS3 file and function, with line numbers from build 25038329.
- `ASCompat.*` calls, `(x : Int)` casts and `Std.string(x)` are conversion helpers. In AS3, write the plain expression.

## Fixes

| # | Bug | Seen in the official game |
|---|-----|---------------------------|
| [01](fixes/01-world-map-entered-early) | Entering the world map quickly crashes | Not confirmed, same code |
| [02](fixes/02-trophy-and-chest-reveal) | Trophy and chest on the same summary crash the reveal | Not confirmed, same code |
| [03](fixes/03-chat-after-hud-refresh) | Opening the chat at the start of a floor crashes after a HUD refresh | Not reproduced, same code |
| [04](fixes/04-power-up-during-revive) | Using a power-up while reviving crashes and spends it | Yes (`TypeError #1009`) |
| [05](fixes/05-swappable-buff-rotation) | Swappable buff rotation crashes | Yes (`RangeError #1125`) |
| [06](fixes/06-ultimate-rampage-victory) | Crash when passing floor 55 in Ultimate Rampage | Not confirmed, same code |
| [07](fixes/07-abandoned-loot-next-dungeon) | Abandoning loot crashes the next dungeon load | Yes (the game freezes) |
| [08](fixes/08-world-map-tooltip-leak) | Each visit to the world map lowers FPS | Yes |
| [09](fixes/09-full-party-friend-links) | Trying to join a full party throws in `setupFriendLinks()` | Yes (`TypeError #1009`) |
| [10](fixes/10-steam-dispose-on-quit) | Quit and the socket error popup skip Steam cleanup | Yes (2 of the 4 exit paths) |

"Same code" means the bug was found in DRH and the AS3 code has the same logic. It can still fail in the official game when the timing lines up, even if it was not reproduced there.

## License

The changes in this repository are released under the [MIT License](LICENSE). The license covers the added and modified lines. The surrounding Dungeon Rampage code shown in the diffs belongs to its rights holders.
