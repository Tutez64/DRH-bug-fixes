# 08. Each visit to the world map lowers FPS

**Symptom:** every town-to-map round trip makes the game slower.

**Cause:** map node tooltips are added to the tooltip layer (107) on `rollOver`. When the camera pans, the matching `rollOut` never comes, so the tooltips stay. `MapTownSubState.destroy()` also never calls `mWorldMap.deinit()`.

**Fix:**

- `UIMapWorldMap.deinit()` empties layer 107.
- `MapTownSubState.destroy()` calls `mWorldMap.deinit()`.

**AS3 location (build 26231687):**

- `UI/Map/UIMapWorldMap.as`, end of `deinit()` (l. 936 to 1049)
- `Town/MapTownSubState.as`, `destroy()` (l. 145)

**Seen in the official game:** yes. 10 to 15 town-to-map round trips roughly halve the FPS.

**Links:** [issue #42](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/42), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/b1319f20370fa3b55b669e0f2a0ef457ca6353f1)
