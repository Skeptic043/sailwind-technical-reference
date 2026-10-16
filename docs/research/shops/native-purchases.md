# How shop purchases establish item ownership

Buying an item changes several things: it leaves shop stock, enters the native save registry, receives item-specific purchase initialization, and is paid for. These steps do not happen at once. In particular, `ShipItem.sold` becomes true before save registration finishes, so it cannot by itself prove that the purchase completed.

[Shops](README.md)

## From shop stock to player ownership

The native purchase path performs these steps:

1. Remove the item from the shop's stock list.
2. Call `ShipItem.Sell`, which sets `sold`, reparents the item to the floating-origin world and updates its look text.
3. Within `Sell`, call `SaveablePrefab.RegisterToSave` to register the actual item instance with the save manager.
4. Optionally pick up the item and call `OnBuy` for its purchase-specific behavior.
5. Subtract currency after `Sell` returns.

`OnBuy` can matter beyond ownership. The fishing rod, for example, activates its bobber, hides the static bobber and resets its joint there. Creating a visible item does not necessarily perform that subtype setup.

## Registration and payment are different facts

`SaveablePrefab.RegisterToSave` uses a private flag to guard registration. When registration proceeds, it allocates an instance ID if the ID is zero, adds the ID to the native ID list, appends the object to `SaveLoadManager`, then sets the flag.

| What you observe | What you can conclude |
| --- | --- |
| `sold` is true | The early sale-state change happened. Registration may still have failed |
| An instance ID exists | An identity was assigned. The object may not be in the current save registry |
| The object is registered | Native registration reached the save manager. Currency may not yet have been deducted |
| A mod record exists | Extension data exists. It still needs to correspond to the correct native item |

In the native path, an exception escaping `Sell` prevents the later currency subtraction. Native registration therefore records ownership progress, not payment completion.

## Why retrying registration can cause trouble

`SaveLoadManager.AddPrefab` appends without deduplication. If registration fails after that append but before setting its private flag, repeating registration can add the same object again. Conversely, `Unregister` removes registry membership without resetting the flag, so calling registration again is not a general way to restore membership.

## Acquiring an item and loading one need different handling

A new acquisition can initialize fresh extension data after valid native ownership. A loaded item may already have saved data that has not reached the mod yet.

During `SaveLoadManager.LoadGame`, native objects are restored before `GameState.modData` is assigned and `LoadModData` runs. An item's early `Awake` callback cannot therefore treat unavailable extension data as proof that no saved record exists.

Saving has its own ordering: `DoSaveGame` captures native prefab data and copies cached item data before `SaveModData`. Data needed by native capture must already be present when that snapshot begins.

For the stages before sale, see [item initialization](../items/spawn-initialization.md) and [vendors, stock and local prices](vendors-stock-and-prices.md).
