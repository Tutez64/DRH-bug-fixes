# 06. Crash when passing floor 55 in Ultimate Rampage

**Symptom:** the game closes right after the team passes the gate at floor 55.

**Cause:** Ultimate Rampage can call `FloorEndingGui.dungeonVictory()` after the victory clip has been torn down. `displayVictory()` then reads `mVictoryRenderer.clip`.

**Fix:** only display the victory when `mVictoryRenderer` is not null.

**AS3 location (build 25038329):** `Floor/FloorEndingGui.as`, `dungeonVictory()` (l. 206)

**Seen in the official game:** not confirmed. The code is the same.

**Links:** [issue #27](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/27), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/b5d62e6f2893d5e3aafb2f0adaa191ca8b98d4e5)
