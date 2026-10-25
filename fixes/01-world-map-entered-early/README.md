# 01. Entering the world map quickly crashes

**Symptom:** entering the world map before its data is loaded (for example right after reaching the town) can crash.

**Cause:** two races on the world map:

- `FRIEND_SCORES_PARSED` rebuilds the map (`deinit()` then `initMap()`), but the `MAP_MENU` navigation layer still points at the old, destroyed node buttons.
- The Infinite Island battle popup assumes the dungeon detail and the local friend scores are already loaded.

**Fix:**

- Add `MenuNavigationController.replaceLayerUIObjects()` and, after the rebuild, point `MAP_MENU` at the new current node button.
- Return early from the II popup when `getInfiniteDungeonDetailForNodeId()`, the GM infinite dungeon or `localFriendInfo` is null.

**AS3 location (build 25038329):**

- `Town/MapTownSubState.as`, `enterState()`, the `FRIEND_SCORES_PARSED` listener (l. 109)
- `Input/MenuNavigationController.as`, new method next to `pushNewLayer()`
- `UI/InfiniteIsland/II_UIMapBattlePopup.as`, l. 148 and `populateChests()` (l. 210)

**Seen in the official game:** not confirmed. The code is the same.

**Links:** [issue #14](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/14) (crashes when moving between menus, one of them on entering the world map), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/d003970ee133fd21e00076434e9b299041426a17)

The DRH commit message says `Fixes #17` by mistake: this fix is for #14, and #17 is fixed by [09](../09-full-party-friend-links).
