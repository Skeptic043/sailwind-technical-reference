# Native storm and wind lifecycle

Sailwind's weather is driven by existing `WanderingStorm` objects and the active region's weather sets. Wind targeting and wave direction have related but separate update paths.

## Storm selection and regional weather

`WeatherStorms.FindClosestStorm` checks its `storms` array, ignores inactive entries and stores the closest active storm plus its distance from the player.

`ApplyStorm` converts that distance to a normalized value and selects among the current region's clear, cloudy, rain and storm weather sets. It copies clear weather outside storm influence and blends between adjacent sets across `cloudyBorder` and `rainBorder`, then calls `Weather.ChangeWeather` with the result.

This makes an existing storm's position meaningful to both weather selection and wind. It does not show that arbitrary new storm objects will be registered or scheduled correctly.

## Wind target inputs

`Wind.SetNewWindTarget` first derives a direction from current base wind, trade wind and regional direction chaos. It chooses a new base magnitude around the current value and clamps that magnitude between `minimumMagnitude` and `maximumMagnitude`.

The method then adds two bounded influences:

- Storm influence is `Mathf.InverseLerp(13000, 500, WeatherStorms.currentStormDistance) * 26`.
- Ocean influence is the base-wind magnitude multiplied by `Mathf.InverseLerp(1500, 4000, GameState.distanceToLand) * 0.66`.

Their combined addition is capped at `20` before the method writes `currentWindTarget`.

`Wind.ForceNewWind` is broader than assigning one vector. It writes both `currentWind` and `currentBaseWind`, then calls `SetNewWindTarget` and `SetNewGustTarget`. The next target and gust can therefore differ from the assigned vector.

## Storm drift and wrap boundary

While the game is playing, `WanderingStorm.Update` translates the storm along normalized `Wind.currentWind` by `Time.deltaTime * 12`. Its timed distance check updates regional activity beyond 12 kilometres. Beyond 44 kilometres, it translates the storm by 1.75 times the horizontal vector from storm to camera, moving it across the player rather than leaving it indefinitely far away.

The 44-kilometre branch means that a storm moved beyond the boundary can be repositioned again by native code rather than remaining indefinitely far away.

## Waves retain separate inertia

`WavesInertia.Update` eases its transform rotation toward the wind transform with an interpolation factor based on `Time.deltaTime`, `directionChangeRate` and `max(10 / currentInertia, 0.25)`. It then calls `UpdateCurrentInertia`.

This is separate from storm placement and wind-target selection. Changing one part of the weather state does not immediately place every other part at its final value.
