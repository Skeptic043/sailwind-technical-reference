# Vanilla item weights

These item masses are game pounds. Most are fixed base weights. Mugs and pots gain contents weight, while the kettle physics path leaves filled kettles at their base weight of 1 pound.

The values come from Sailwind 0.39.2 registered prefabs and native mass calculations.

[Items](README.md)

## Selected item weights

| Item | Pounds | Registered prefab indices and conditions |
| --- | ---: | --- |
| Kettle, all three regions | 1 | 382-384, including filled state |
| Small pot | 1 | 156, empty |
| Large pot | 2 | 157, empty |
| Wall hook / lamp hanger | 0.5 | 79 |
| Fishing rod | 1 | 95 |
| Fishing hook item | 0.1 | 99, per loose item object |
| Al'Ankh bed | 25 | 60 |
| Emerald Archipelago bed | 16 | 61 |
| Aestrin beds | 28 | 62 and 63 |
| Oar | 2.2 | 168 |
| Standard lanterns, including Emerald colors | 2 | 110-114 and 134 |
| Large Aestrin lantern | 3 | 133 |
| Mug, wood/clay/metal/gold | 0.2 | 100-103, empty |
| Compass, all three regions | 1 | 80-82 |
| Sun compass | 1 | 86 |
| Firewood | 0.5 | 71, per piece |
| Sealed firewood crate | 11 | 108: 5 container + 12 pieces at 0.5 |
| Sealed fishing-hook crate | 4.2 | 104: 3 container + 12 item objects at 0.1 |

Values are rounded only to remove float32 display artifacts. The hook item itself has `amount = 12`, but `ItemRigidbody.UpdateMass` does not multiply a loose hook's base mass by that amount. The crate formula multiplies by the crate's own contained-object count. The registered prefab has 12 contained objects, so its sealed-crate total is 4.2 pounds.

## Contents and unit interpretation

`MissionDetailsUI.GetCargoWeight` returns the same serialized base/contents mass used for cargo. `MissionDetailsUI.UpdateTexts` multiplies by shipment count and appends `pounds`, without converting kilograms. These values are pounds, not kilograms.

`ItemRigidbody.UpdateMass` starts at `ShipItem.mass`, then adds:

- Crates: contained prefab's `ShipItem.mass * crate.amount`.
- Bottles, mugs, buckets and liquid barrels (`ShipItemBottle`): `health`, the current liquid amount.
- Tea/coffee containers (`ShipItemTea`) and salt barrels (`ShipItemSalt`): `amount * 0.1`.
- Soup pots (`ShipItemSoup`): `currentWater + currentEnergy / 20 + currentUncookedEnergy / 20`.

Thus a mug holds up to 3 liquid units and weighs 3.2 pounds full. Small and large pots hold 10 and 20 water units and weigh 11 and 22 pounds when filled only with water. Stew adds the food-energy terms. Kettle capacities are 5, but `ShipItemKettle : ShipItem` satisfies none of these additive checks and does not override this calculation. Its pour/fill methods call `UpdateMass` without adding its water or tea. Whether that omission is intentional is unknown.

`BoatMass.UpdateMass` skips item bodies with a non-null `GetCurrentInventorySlot()`. These item values therefore do not imply that every packed object contributes this much to boat load. Opened inventory containers and sealed crate contents use different accounting paths. The catalog reports the sealed prefab formula, not a packed inventory crate total.

## More item weights

| Item | Pounds | Conditions |
| --- | ---: | --- |
| Hammer, slicing knives, spyglasses, quadrant, chip logs, broom, ink set | 1 each | Base weights |
| Maps/charts | 1.5 each | Registered foldable map items |
| Wind chimes | 0.7 | Metal and bamboo |
| Bucket | 0.65 / 9.65 | Empty / full of 9 liquid units |
| Water bottle | 0.5 / 5.5 | Empty / full of 5 liquid units |
| Water barrel | 8 / 68 | Empty / full of 60 liquid units |
| Tea or coffee box | 5 | 2 container + 30 portions at 0.1 |
| Coffee barrel | 8 | 2 container + 60 portions at 0.1 |
| Salt barrel | 17 | 7 container + 100 portions at 0.1 |
| Oakum | 9 | Base weight, `amount = 200` does not add mass here |
| Brining jar | 6 | Base weight only |
| Small tables | 12 | Regional variants |
| Large tables | 20 / 18 / 25 | Al'Ankh / Emerald / Aestrin |
| Chairs | 6 / 6 / 8-10 | Al'Ankh / Emerald / Aestrin |
| Wall shelves | 8 | Three regional variants |
| Stove | 80 / 30 / 50 | Al'Ankh / Emerald / Aestrin |
| Smoker | 100 / 50 / 50 | Al'Ankh / Emerald / Aestrin |
| Drying rack | 18 / 22 / 24 | Al'Ankh / Emerald / Aestrin |

## Full catalog

[The full catalog](vanilla-item-weight-catalog.md) lists all registered item records with base and serialized prefab totals.
