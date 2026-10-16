# Native item initialization and paused spawning

A newly created item needs more than a visible object. Sailwind sets up its item rigidbody and subtype state through several callbacks, and some of that setup can run while the game is paused. Waiting a fixed number of frames is not proof that the item is ready for interaction or physics.

[Items](README.md)

## Initialization begins before normal physics

`ShipItem.Awake` starts `LoadAfterDelay` immediately. Despite the coroutine's name, it creates the separate `ItemRigidbody` before its first yield. Native `Start` and `OnLoad` then perform subtype setup, including crate initialization.

The useful question is whether the required state exists, rather than how many render frames have passed. Different items can need different setup beyond the common `ShipItem` path.

`ShipItem.Awake`, `LoadAfterDelay` and the inherited `GoPointerButton.Start` do not hide the root renderer while physics is pending. An item can therefore be visible before every relevant initialization step is complete.

## Initialization during pause

Native Start/end-of-frame subtype initialization can proceed during pause, while movement resumes after unpause.

A kinematic rigidbody is not necessarily fixed scenery. `ItemRigidbody.FixedUpdate` can make bodies kinematic during their first six physics frames, while attached or nailed, or under debug pause. `ItemRigidbody.Start` and `CreateSubcollider` also place native bodies on layer 2.

Kinematic state alone therefore does not distinguish loose objects from fixed scenery during pause. `ItemRigidbody.GetShipItem`, the `attached` state, `ShipItem.nailed` and `BoatDamage` ancestry expose more of the native distinction between loose items, attachments and boat structure.

## Initialization does not establish ownership

Creating the rigidbody or finishing subtype setup does not prove that native save registration or purchase-specific callbacks have completed. Those stages have their own ordering, explained in [shop purchases and item ownership](../shops/native-purchases.md).

## Version scope

This initialization path comes from a September 2026 Sailwind beta and has not been rechecked against the current reference version.
