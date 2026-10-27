# 10. Quit and the socket error popup skip Steam cleanup

**Symptom:** leaving through the Quit button or the socket error popup never runs `DungeonBustersProject.onExit()`, so `mSteamworks.dispose()` is skipped.

**Cause:** `onExit()` listens to `NativeApplication`'s `exiting` event, but AIR's `NativeApplication.exit()` does not dispatch it. `exiting` only fires when the exit comes from outside the game.

| Exit path | Official game | With the fix |
|-----------|---------------|--------------|
| Window close button | `exiting` fires, Steam is disposed | Unchanged |
| OS-initiated exit (shutdown, logoff, Cmd+Q) | `exiting` fires, Steam is disposed | Unchanged |
| Quit button (`UIExitPopup`) | `exit()` only, Steam is not disposed | Steam is disposed |
| Socket error popup (`SocketErrorState`) | `exit()` only, Steam is not disposed | Steam is disposed |

**Fix:**

- Add `DBFacade.exitApplication()`: dispatch a cancelable `Event.EXITING` on `NativeApplication`, then call `exit()` unless it was cancelled.
- Use it in `UIExitPopup` and `SocketErrorState`.
- Make `onExit()` run once, in case `exiting` arrives twice.

**AS3 location (build 26231687):**

- `DungeonBustersProject.as`, `onExit()` (l. 107)
- `Facade/DBFacade.as`, new method
- `UI/Popup/UIExitPopup.as`, `exitApplicationAction()` (l. 56)
- `StateMachine/MainStateMachine/SocketErrorState.as`, `errorDialogResponce()` (l. 40)

**Seen in the official game:** yes, on the two in-game exit paths. The other two already work.

**Links:** [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/b8825a0ed2d36974ada9529603826c98fbcf11b3)
