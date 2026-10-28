# 11. Leaving an interface after swapping heroes gets the game stuck

**Symptom:** select a dungeon, swap heroes from its battle popup, then go back to the town and open another interface (Storage, Tavern, etc.). Leaving it with its close button makes the button disappear and throws an error, and `Escape` throws another one. The game has to be restarted.

Close button:

```
UncaughtError: TypeError: Error #1009
    at Function/UIMapBattlePopup.as$6:anonymous()
    at Input::MenuNavigationController/onKeyPressed()
    at Brain.Input::InputManager/onKeyDown()
```

`Escape`:

```
UncaughtError: TypeError: Error #1009
    at Brain.UI::UIButton/onRollOver()
    at Brain.UI::UIObject/onFocused()
    at Brain.UI::UIButton/onFocused()
    at Brain.UI::UIObject/setFocused()
    at Input::MenuNavigationController/setFocusedUiObject()
    at Input::MenuNavigationController/popLayer()
    at Town::InventoryTownSubState/exitState()
    at Brain.StateMachine::StateMachine/transitionToState()
    at Town::TownStateMachine/enterHomeState()
    at Town::TownHeader/determineCallback()
    at Input::MenuNavigationController/onKeyPressed()
    at Brain.Input::InputManager/onKeyDown()
```

**Cause:** back from the tavern through Swap, the map reopens the dungeon's battle popup, whose `UI_MAP_BATTLE_POPUP` layer sits above `MAP_MENU`. Leaving the map then destroys the popup (`mWorldMap.deinit()`) without popping its layer, so `popLayer("MAP_MENU")` is refused. The stale layer stays on top, and leaving the next state focuses the popup's destroyed Battle button.

**Fix:** add `MapTownSubState.popBattlePopupLayer()`, which pops `UI_MAP_BATTLE_POPUP` when it is the top layer, and call it before `mWorldMap.deinit()` in `exitState()` and in the `FRIEND_SCORES_PARSED` listener.

**AS3 location (build 26231687):** `Town/MapTownSubState.as`, the `FRIEND_SCORES_PARSED` listener in `enterState()` (l. 109) and `exitState()` (l. 124). The listener in the diff already includes [01](../01-world-map-entered-early)'s change.

**Seen in the official game:** yes, with the errors above.

**Links:** [issue #46](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/46), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/4a41082d076af95c2d4f291e8127e340a496a5c8)
