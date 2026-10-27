# 03. Opening the chat at the start of a floor crashes after a HUD refresh

**Symptom:** with the `want-hud-refresh-on-floor-transition` feature flag on, opening the chat right at the start of a new floor can crash.

**Cause:** `UIChatLog.destroy()` destroys the slider and the chat log button, but not `mChatInputText` and `mChatSendButton`. The old input stays alive after the HUD rebuild and later touches its destroyed text field.

**Fix:** destroy and null `mChatInputText` and `mChatSendButton` in `destroy()`.

**AS3 location (build 26231687):** `UI/UIChatLog.as`, `destroy()`, after `mSlider = null` (l. 439)

**Seen in the official game:** not reproduced. The code is the same. The flag is off by default (`Brain/Utils/FeatureFlags.as`).

**Links:** [issue #19](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/19), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/4c6b2568606f6a206221e75441205f780c1f7831)
