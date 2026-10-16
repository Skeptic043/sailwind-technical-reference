# Native interior and listener audio

Sailwind's interior audio state follows the player. Interior triggers update one global indoor flag and transition the active mixer snapshot, while configured doors decide whether the result is fully or partly indoors. This is useful listener state, but it is not a general model of where every sound source is located.

[Audio](README.md)

## Entering and leaving an interior

`InteriorEffectsTrigger.OnTriggerEnter` responds to a collider tagged `Player`, sets `GameState.indoors` and calls `FadeToInterior`. `OnTriggerExit` clears the flag and calls `FadeToExterior`.

The global flag therefore describes the player's current environment. `Rain.Update` uses it when deciding whether rain-splash particles should run, which shows that the same state can affect systems outside the mixer transition itself.

## Doors choose the interior snapshot

`InteriorEffectsTrigger.FadeToInterior` checks its configured `GPButtonTrapdoor`, `GPButtonHouseDoor` and house-door array references. If any checked door is open, the trigger sets `semiIndoor` and transitions to `AudioMixers.semiIndoorSnapshot`. Otherwise it transitions to `AudioMixers.indoorSnapshot`. Both paths use a two-second transition.

Leaving the trigger calls `FadeToExterior`, which returns the listener to the exterior mix. `AudioMixers` also exposes separate game-active and sleep snapshots, so interior state is only one part of the native mixer state machine.

## Listener state is not source membership

`GameState.indoors` is a listener-oriented global signal. It does not identify which interior volume contains an emitter, whether an emitter is on the other side of a closed door, or how overlapping interior triggers resolve.

Serialized scenes contain both box and convex-mesh interior trigger volumes. Active listener identity still depends on the enabled component and active hierarchy at runtime, so serialized hierarchy alone does not establish which listener is active.

## Version scope

The managed behavior was inspected in Sailwind 0.39.2. The box and convex-mesh interior-trigger observations come from an earlier beta scene.
