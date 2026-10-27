# 05. Swappable buff rotation crashes

**Symptom:** the game crashes while rotating the swappable buff icons on the HUD.

```
UncaughtError: RangeError: Error #1125
    at Actor.Buffs::BuffHandler/processSwappableBuffDisplays()
    at Brain.WorkLoop::WorkLoopManager/processDoLaters()
```

**Cause:** `processSwappableBuffDisplays()` is scheduled with `doLater` and assumes at least two swappable buffs, each with a live `buffView`. A buff can be removed before it runs, which leaves `mCurrentSwappableBuff` out of range.

**Fix:** return (and reset the index) when there are fewer than two buffs, clamp an out-of-range index, and skip entries without a `buffView`.

**AS3 location (build 26231687):** `Actor/Buffs/BuffHandler.as`, `processSwappableBuffDisplays()` (l. 214)

**Seen in the official game:** yes, with the error above.

**Links:** [issue #26](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/26), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/16ee240230770a61a82441ed7aafbad8497356af)
