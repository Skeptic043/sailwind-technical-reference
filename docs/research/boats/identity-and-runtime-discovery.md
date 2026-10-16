# Boat identity and runtime discovery

A Sailwind boat does not have one universal identity. Its native save index participates in save registration, its loaded object represents the live boat and its purchase flag records native ownership state. These values answer different questions and are not interchangeable.

[Boats](README.md)

## Native save identity

`SaveableObject.Awake` checks its `sceneIndex`, reports a conflict when that slot already has an object, then registers through `SaveLoadManager.AddObject` unless the component is already marked registered. `AddObject` assigns the object directly into `SaveLoadManager.currentObjects[sceneIndex]`.

A conflict message is therefore not a rejection. A later registration can occupy the same array slot. Duplicate indexes are an identity collision. The message does not establish that either object was destroyed or safely ignored.

The index alone is not a complete description of the object. It identifies a save slot, not a hull type, berth or purchase state.

## Purchase state is a separate question

`PurchasableBoat.Awake` obtains the boat's `SaveableObject`. Its `isPurchased` method reads `SaveableObject.extraSetting`. That flag answers whether the native purchase state is set. It does not provide a unique boat identity, berth, size or safe-placement result.

Unsold boats remain live objects that can occupy space and own references. Conversely, a purchased flag does not prove that the boat is initialized, loaded at a usable berth or safe to move.

## Registered and loaded are different states

`SaveLoadManager.currentObjects` records save registration. Loaded boat objects and components represent what currently exists in the scene. An object can exist before registration finishes, and a serialized prefab does not establish either live state.
