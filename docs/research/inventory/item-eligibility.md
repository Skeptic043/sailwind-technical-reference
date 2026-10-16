# Player inventory eligibility

An item belongs to Sailwind's player-inventory category when it has a `ShipItem`, is not marked `big`, and is not a bottle whose capacity exceeds 10. Purchase state and an empty slot decide whether one insertion can happen now. They are not category membership.

## The native insertion path

`GPButtonInventorySlot.OnItemClick(PickupableItem)` follows these checks:

1. An occupied slot returns false.
2. The held object is resolved to `ShipItem` and its `sold` state is checked.
3. The insertion branch requires the item not to be `big`.
4. A `ShipItemBottle` enters only when `GetCapacity()` is at most 10.
5. Eligible input reaches `GPButtonInventorySlot.InsertItem`.

`sold` and free-slot state are transient. Using either in a reusable category test would make the same physical item change categories when it is purchased or when another slot is emptied.

The method's return value is also not a pure eligibility result. Some paths that do not call `InsertItem` still return true. `InventoryHotkeys.Update` uses a true return to call the held item's drop path and release the pointer, so invoking `OnItemClick` just to classify an item can change state.

## Category rule and insertion state

The category rule depends on these stable properties:

```text
has ShipItem
and not big
and either not ShipItemBottle or capacity <= 10
```

The actual insertion decision remains separate. The native method still handles slot occupancy, purchase state, `ItemRigidbody.EnterInventorySlot`, slot assignment and UI sound.
