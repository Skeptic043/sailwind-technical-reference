# Native equipment registry and mod-added items

Sailwind reconstructs saved items from templates in `PrefabsDirectory`. Mod-added equipment can participate in that native path when its provider registers a complete template before the directory's derived caches are populated.

[Mod discovery](README.md)

## Directory and derived caches

`PrefabsDirectory.Start` sets the singleton and calls `PopulateShipItems`. That method creates a `shipItems` array with the same length as `directory` and caches each entry's `ShipItem`. Startup then validates each directory entry's `SaveablePrefab.prefabIndex` against its array position.

`PrefabsDirectory.GetItem(index)` checks `directory[index]` for null but returns `shipItems[index]`. Other native methods also read `directory` directly. This means a provider that changes one collection after cache population can leave different native consumers with different views.

## Identity and reconstruction

`SaveLoadManager.LoadGame` and `BoatLocalItems.SpawnCachedItems` select templates through saved prefab indices. Those indices describe positions in the current native directory. They can move when providers, load order or the game change, so a runtime index is not a stable provider identity by itself.

A registered template also does not establish that the resulting item is usable. Visuals, colliders, physics components, `ShipItem` behavior and any provider-specific initialization remain separate parts of the template's contract. Category or a cloned vanilla donor cannot establish those requirements for an arbitrary custom item.
