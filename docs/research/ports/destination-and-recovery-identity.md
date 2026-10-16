# Destination, start-site and recovery identity

A Sailwind destination is not one interchangeable object. A registered `Port` provides the native port identity. A `RecoveryPort` supplies a recovery berth linked to a parent port. A selectable start site, shipyard or geometric marker can exist without creating either one.

[Ports](README.md)

## Distinct identity roles

`Port.Start` registers the live object in `Port.ports` by `portIndex`. `Port.GetPortName` returns its serialized name. That identity is used by trade, missions and saved last-port state, but it does not identify a particular dock or prove that a hull fits there.

`RecoveryPort.Start` reparents its object to the shifting world and registers it in the separate `Recovery.ports` list. `Recovery.GetPort` searches those entries by their parent port's index. A port can therefore be registered without having a complete recovery berth, and more than one site can belong to the same native port.

A shipyard is another capability, not another port identity. A selectable start site can refer to a native port, but that metadata does not register a new `Port` or `RecoveryPort`.

## A geometric marker is not a recovery registration

Serialized scenes can contain disabled or inactive transforms that are useful as geometric references. They can provide an origin and heading after their owning island is loaded. They do not by themselves provide an initialized recovery berth, usable mooring springs, safe water depth or dry shore.

The Sailwind 0.39.2 scene illustrates both cases. Saffron Island has registered Port 33, but its attached recovery component is incomplete and a separate dormant marker does not supply local usable cleats. The Pirate Hideout island has authored cleats but no `Port` or `RecoveryPort` in that scene.

## Last visited means native proximity, not docking history

`RecoveryPort.Update` follows a timed distance check and writes its `parentPort` to `GameState.lastVisitedPort`. The method does not require a connected mooring rope or grounded player before that write. Native save loading restores the saved port index through `Port.ports`.

The value is a remembered recovery destination rather than an exact record that the player docked, stepped ashore or used a shipyard.

## Live identity boundaries

Current native identity comes from the live port and recovery registries together with scene lifetime and island ancestry. Asset presence is not live registration. An inactive authoring object is not a loaded destination. A port name or region is not a berth.
