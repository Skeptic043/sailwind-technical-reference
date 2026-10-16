# Mooring source ownership

A stored spring reference records the spring a mooring rope used earlier. The live connected body records the body currently attached to that spring. These values can diverge.

[Placement](README.md)

## What native mooring records

`PickupableBoatMooringRope.MoorTo` assigns the dock `SpringJoint.connectedBody` to the rope's boat rigidbody, configures the spring and stores that spring in the rope's private `mooredToSpring` field. It then parents the rope under the spring.

`IsMoored` checks whether `mooredToSpring` exists. It does not compare the spring's current connected body with the rope's boat.

`Unmoor` also begins from the retained spring. It does not read `SpringJoint.connectedBody` before clearing the connection and resetting the spring. If the reference becomes stale and the spring is later connected to another boat, the original rope still retains that spring reference.

`BoatMooringRopes.UnmoorAllRopes` calls `Unmoor` and resets each rope, so the same behavior also appears in the native bulk-release path.

## An anchor is not a dock line

`BoatMooringRopes.AnyRopeMoored` returns true when the boat's anchor is set before it checks the mooring ropes. Its name therefore means the boat has some native hold, not necessarily that it is tied to a dock.

Dock-line state therefore depends on `PickupableBoatMooringRope.IsMoored` plus the live connected body. Anchoring state remains separate.
