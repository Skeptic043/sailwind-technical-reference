# Item identity and restoration

Sailwind uses two different identities for a saved item. `SaveablePrefab.prefabIndex` selects the template used to reconstruct it. `SaveablePrefab.instanceId` identifies that particular acquired or loaded instance. Extensions that merge those roles can reconstruct the wrong template or associate state with the wrong acquired instance, while a large prefab index does not establish a reserved namespace.

[Persistence](README.md)

## Template identity chooses what gets created

`SaveablePrefab.PrepareSaveData` records both `prefabIndex` and `instanceId`. During `SaveLoadManager.LoadGame`, the saved prefab index is used to index `PrefabsDirectory.directory` directly. The loader either stores the record with its parent boat or instantiates the selected template and calls `SaveablePrefab.Load`.

This has two consequences:

- A custom prefab slot must be registered before every native load path can read it.
- An absent or out-of-range slot can fail before extension data is available. A slot occupied by a different template can reconstruct the wrong object.

`PrefabsDirectory` also maintains item-facing data derived from the directory. Correct registration therefore depends on the directory's construction order and all relevant collections. Changing one array is not established as sufficient.

## Instance identity follows the saved object

`SaveablePrefab.Load` restores the saved `instanceId` before it calls `RegisterToSave`. For a new object whose ID is zero, native registration allocates an ID, adds it to the native ID list and adds the object to the current save manager.

New-acquisition defaults remain separate from restoration. An early `Awake` callback cannot conclude that extension data is missing because `SaveLoadManager.LoadGame` reconstructs native items before it assigns `GameState.modData` and calls `LoadModData`.

For the acquisition side of this distinction, see [how shop purchases establish item ownership](../shops/native-purchases.md).

## Cached boat items are still part of the identity set

`BoatLocalItems.CacheCurrentItems` copies native records with `PrepareSaveData`. `SpawnCachedItems` later selects `PrefabsDirectory.directory[cachedItem.prefabIndex]`, creates a new object and calls `Load` with the cached record.

A cached record therefore remains part of the saved identity set even when no matching live `GameObject` exists. Its instance ID can still correspond to state that must be restored when the boat's items are spawned again.
