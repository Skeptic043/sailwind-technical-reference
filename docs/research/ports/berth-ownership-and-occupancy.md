# Berth ownership and occupancy

A Sailwind berth has both authored associations and live spring state. A boat's dock references can identify its intended endpoints, while a spring's connected body identifies the body currently attached to that spring. Neither value alone describes the whole berth.

[Ports](README.md)

## Association answers where a berth belongs

Stock boats use front and back dock references to anchors under the same island and port ancestry. That pairing is stronger than using the boat's world position or broad region because it retains semantic ownership when the shifting world moves.

The front and back dock references, together with their island or port ancestry, identify the authored association more precisely than proximity alone. A nearby cleat, matching region or short distance does not establish the intended berth.

Authored references can also be shared by inactive alternatives. One active stock boat shares dock references with inactive variants in the earlier scene record. An authored claim is therefore not the same thing as active occupancy.

## Occupancy answers who owns it now

At runtime, a dock's `SpringJoint.connectedBody` is the direct record of current spring ownership. A null connected body does not prove that the berth is unoccupied. The spring may not have initialized, a boat may still have authored references to it or a hull may physically occupy the space without an active connection.

Purchased and unsold boats can both occupy berth geometry. Purchase state therefore does not replace either authored association or live spring state.

For the rope-side behavior, see [Mooring source ownership](../placement/mooring-source-ownership.md). Authored references describe intended association. Initialized springs and occupied geometry describe actual occupancy and clearance.

## Version scope

The live spring path reflects Sailwind 0.39.2. The authored-association examples come from an earlier scene record and are not a current berth inventory.
