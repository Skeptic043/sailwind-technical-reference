# How vendors, stock and local prices fit together

A shop can display an item and still be unable to sell it. Native item setup, shop association, purchase ownership and save registration are separate parts of the process. A working native shop connects physical goods, a keeper and the local economy through runtime relationships that are not established merely by placing an item beside an NPC.

These relationships come from two September 2026 Sailwind beta builds and have not been re-established for Sailwind 0.39.2.

[Shops](README.md)

## What makes the stock part of a shop

`ShopArea` maintains the stock list. `Shopkeeper` depends on a home transform, a direct `IslandSceneryScene` parent, a region and a loaded island economy. Some links are populated at runtime, which means a scene-file search can find likely vendors without proving which keeper owns a particular stock area.

`ShopArea.OnTriggerEnter` consumes `itemsForSale`, while `ShopArea.Awake` requires a Rigidbody. Those runtime dependencies are separate from the scene objects' visual proximity. A nearby keeper or visible stall does not by itself establish the native association.

## Unsold stock is different from an owned item

`ShopItemSpawner` owns one unsold child instance. It spawns while near the player or during startup, hides non-night stock at night, and waits 120 simulation seconds to restock after sale. That unsold stock is not saved as a player item.

Displaying unsold goods therefore does not prove that a purchase can complete. The sale has to move an item from shop stock into native ownership, with its own registration and payment boundaries. See [how purchases establish ownership](native-purchases.md) for that transition.

## How the keeper calculates a local price

For unsold stock, `Shopkeeper.GetPrice` uses three pieces of information:

1. The item's `ShipItem.value`.
2. The immediate parent `ShopItemSpawner.priceMult`.
3. The retail reputation discount for the keeper's region.

`GetLocalPrice` then converts the result through `CurrencyMarket`. A missing or incorrect region can affect hover pricing as well as the sale.

The currency indices are:

| Index | Currency |
| --- | --- |
| 0 | Al'Ankh Lions |
| 1 | Emerald Dragons |
| 2 | Aestrin Crowns |
| 3 | Gold Lions |

`CurrencyMarket.GetExchangeRate(sell,buy,false)` returns `currentPrices[buy] / currentPrices[sell]`. Local shop pricing uses currency conversion without the separate money-exchange fee. The exchange UI applies a 1 percent fee.

Scene-file rates are starting values. `CurrencyMarket.MarketCycle` changes the first three rates daily, currency trades also change rates, and saved games retain and restore them. Calculating a price from serialized defaults therefore does not establish the price in an existing save.

Base prices are integers, so a desired converted price may also involve rounding. The current rates and native pricing path determine what a player should see.
