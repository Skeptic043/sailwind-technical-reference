# Buoyancy and spawn height

A fixed vertical offset cannot place every Sailwind boat at its floating draft. Native buoyancy depends on the boat's initialized force points, rigidbody mass and current sea level.

[Physics](README.md)

## Initialized force points

`Crest.BoatProbes.Start` applies `customScale` to the serialized force-point offsets, obtains the rigidbody, calculates the total point weight and allocates its query arrays. Multiplying the initialized offsets by `customScale` again would apply that scaling twice.

`UpdateWaterQueries` adds the stored center-of-mass Y offset to each force-point offset and transforms that point into world space. `FixedUpdateBuoyancy` compares each query point with sea level plus sampled wave displacement, then applies upward force at submerged points according to point weight, `_forceMultiplier` and `_totalWeight`.

For a world-up pose in flat water, let `o_i` be each initialized point's world Y offset from the root and `w_i` its weight. A root height `Y` supports this mass:

```text
supported mass = 1000 * forceMultiplier / sum(w_i)
                 * sum(w_i * max(0, seaLevel - Y - o_i))
```

The flat-water balance height is the value of `Y` where supported mass equals the rigidbody mass. Because emerged points contribute no lift, the active set of force points can change as the hull moves vertically.

## The height changes with the boat and load

The Sailwind 0.39.2 Kakam assets provide a concrete example. With authored mass 950, force multiplier 1.9 and a scaled probe Y offset of 0.1, the flat-water equation places the root at approximately `-0.6000` when sea level is zero.

The equilibrium estimate changes with live mass when the boat carries cargo or different fittings. An authored spawn height can instead represent a presentation or berth choice.

## What the balance height represents

The force-balance solution answers where a world-up hull balances in flat water. It does not choose a berth, avoid shore or other boats, establish rope reach, preserve a departure heading or guarantee that the boat remains stable.

Sea level and moving-frame transforms can change across a scene transition. The equation does not model waves, angular response, collision clearance or vertical settling.
