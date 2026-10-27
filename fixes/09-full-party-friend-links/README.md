# 09. Trying to join a full party throws in `setupFriendLinks()`

**Symptom:** after trying to join a full party, the game sends you back to the world map, as expected. A few seconds later, this error is thrown:

```
UncaughtError: TypeError: Error #1009
    at Town::HomeState/setupFriendLinks()
    at Brain.Workloop::WorkLoopManager/processDoLaters()
```

**Cause:** `HomeState.setupState()` runs from the `TownSubState` constructor and schedules `setupFriendLinks()` 2 s later. That call runs even if this `HomeState` never becomes current, as when the game goes straight to the world map. It then reads `mUILeaderboard`, which is only set in `enterState()`.

**Fix:** schedule `setupFriendLinks()` from `enterState()` instead of `setupState()`. `exitState()` already cancels the task.

**AS3 location (build 26231687):** `Town/HomeState.as`, `setupState()` (l. 294 to 298) and `enterState()` (l. 828)

**Seen in the official game:** yes, with the error above. The game keeps running.

**Links:** [issue #17](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/17), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/fa276026454836463c6d19569dc715cbd77463a1)
