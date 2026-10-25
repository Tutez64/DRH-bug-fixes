# 04. Using a power-up while reviving crashes and spends it

**Symptom:** using a power-up during another animation (notably reviving) throws in `startCooldown()`, and the power-up is consumed anyway.

```
UncaughtError: TypeError: Error #1009
    at Combat.Weapon::WeaponController/startCooldown()
    at Combat.Weapon::WeaponController/attack()
    at Combat.Weapon::ConsumableWeaponController/consume()
    at Combat.Attack::PlayerOwnerAttackController/tryToDoConsumableAttack()
```

**Cause:** `ConsumableWeaponController.consume()` spends the item without checking whether the attack started. `WeaponController.attack()` then starts the cooldown even though no attack timeline is playing.

**Fix:**

- `attack()` returns a `Boolean`: `false` on early returns (not enough mana, no timeline), otherwise whether the attack actually started.
- The cooldown only starts when the attack started.
- `consume()` only spends the item when `attack()` returns `true`.

**AS3 location (build 25038329):**

- `Combat/Weapon/ConsumableWeaponController.as`, `consume()` (l. 17)
- `Combat/Weapon/WeaponController.as`, `attack()` (l. 90 to 135)

**Seen in the official game:** yes, with the error above.

**Links:** [issue #22](https://github.com/Tutez64/Dungeon-Rampage-Haxe/issues/22), [DRH commit](https://github.com/Tutez64/Dungeon-Rampage-Haxe/commit/824b1ab69cb1054b5d5d7a7b4784a81335919004)
