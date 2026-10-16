# Boat mass and load limits

Sailwind has no universal stored cargo-capacity value. The useful static boundaries come from native mass accounting, buoyancy force points and the height of enabled overflow checks. They can compare hulls under one model, but they are not proven maximum sailing loads.

[Physics](README.md)

## Native load estimates

For Sailwind 0.39.2, these figures are pounds of additional cargo and loose supplies after the modeled default hull, active fittings, counted starting sails and one 160-pound player. Values are rounded to the nearest pound.

`Upright first overflow` is the added load at which the lowest enabled overflow check reaches flat water with no heel. The second column applies a prescribed 15 to 20 degree heel and keeps the limiting check 0.5 metres above flat water. It reports the range across that angle interval, not a normal working rating.

| Native boat | Upright first overflow | 15 to 20 degree heel with 0.5 m clearance |
| --- | ---: | ---: |
| Dhow | 2,447 | 633–914 |
| Cog | 3,285 | 237–753 |
| Kakam | 2,256 | 219–538 |
| Sanbuq | 22,722 | 6,058–9,158 |
| Junk | 16,230 | 3,291–5,644 |
| Brig | 56,434 | 14,205–22,342 |
| Jong | 65,805 | 16,686–26,307 |
| Baghlah | 88,528 | 39,581–49,149 |

The Cog illustrates why the second column is not a simple boat ranking. It has more upright headroom than the Dhow, but its overflow checks sit farther outboard. Prescribed heel lowers the limiting opening faster, so the stricter model gives the Cog less remaining allowance.

## What native mass accounting includes

`BoatMass.UpdateMass` begins with `selfMass` and `partsMass`. It adds the rigidbody mass of tracked aboard items whose current inventory slot is null, active fitting-option mass, counted sail mass and 160 pounds when the player is aboard that boat. It then writes the total to the boat rigidbody and updates the center of mass.

An item inside an inventory slot is skipped by this loop. Its prefab mass is therefore not automatically another contribution to the boat total through `itemsOnBoat`. Loose cargo, carried items and active fittings contribute according to their actual state.

The game's mission UI displays cargo mass followed by `pounds`, with no conversion in that path. The [item weight reference](../items/vanilla-item-weights.md) supplies the native values needed to deduct loose native supplies from an allowance.

## First overflow is not sinking

`WaveSplashZone.Update` samples the water height at an overflow object, applies that object's `verticalOffset` and calls `BoatDamage.Overflow` when the water reaches the opening. The first contact starts an intake path. It is not an instantaneous sunk-state threshold.

`BoatDamage.UpdateWaterAndDrag` also drains water while the applicable native conditions hold. Above half-full bilge, it begins reducing the buoyancy multiplier. At a full bilge it records the sunk state and continues reducing buoyancy. Damage, drainage, waves and the duration of immersion therefore matter after first contact.

The model deliberately stops at a clearly defined geometric boundary. It does not solve cargo-induced trim, pitch, sail-induced roll, wave spectra, collision response, useful speed or storm survival.

## Interpretation of the estimates

The table supports comparison and planning rather than a maximum-load claim. Dense cargo position, balance, route and sea state remain outside the calculation, and changes to hull geometry, force points, overflow objects, default fittings, starting sails or native mass code change the estimates. Handling, speed, roll response, flooding recovery and a practical working load still depend on the boat's live configuration and conditions.
