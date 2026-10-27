# 07. Abandoning loot crashes the next dungeon load

**Symptom:** leaving the victory screen without acting on a chest (abandoning the loot) makes the next dungeon load crash (DRH) or freeze (official game).

**Cause:** `DBUIPopup.close()` runs the callback before `destroy()` pops the popup's navigation layer. The abandon-loot confirm's callback pops `DISTRIBUTED_DUNGEON_SUMMARY` while the confirm's own layer is still on top, so `popLayer()` refuses ("You tried to pop a layer that wasn't currently the top one"). A stale `UIObject` stays referenced and breaks the next teardown.

**Fix:** in `close()`, pop the popup's navigation layer and clear `mUILayerName` before running the callback. `destroy()` then skips the pop.

**AS3 location (build 26231687):** `UI/Popup/DBUIPopup.as`, `close()` (l. 235)

**Seen in the official game:** yes. The game does not crash or log an error, but it freezes and has to be restarted.

**Links:** [issue #41](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/41), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/b160c8c7a572a7c949792a14e7cca31fc4a26d5f)
