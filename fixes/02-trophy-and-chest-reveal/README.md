# 02. Trophy and chest on the same summary crash the reveal

**Symptom:** getting a trophy and a (rare) chest in the same dungeon crashes on the summary screen.

**Cause:** reveal state 25 hides the trophy panel with `mDungeonAchievementPanelMovieClipRenderer.stop()` and `.clip.visible = false`. By then, the chest reveal may have destroyed that renderer.

**Fix:** only stop and hide the panel when the renderer and its clip still exist. The state machine still moves on to state 30.

**AS3 location (build 25038329):** `DistributedObjects/DistributedDungeonSummary.as`, `updateRevealState()`, `case 25` (l. 600)

**Seen in the official game:** not confirmed. The code is the same.

**Links:** [issue #18](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/18), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/a7f18b0e8e51412832bae0d11f8d9cbf2a738339)
