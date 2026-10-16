# Lantern fuel and light state

`ShipItemLight.ExtraLateUpdate` handles fuel subtraction, clamps depletion, turns the light off and applies the lantern-shadow setting.

## Native depletion

The callback runs its fuel path only while `ShipItemLight.on` is true. It subtracts:

```text
Time.deltaTime * Sun.sun.timescale / 50 * fuelConsumptionRate
```

from `ShipItem.health`. At zero or below, it clamps health to zero and calls `SetLight(false)`. The remainder of the callback applies `Settings.lanternShadows` to the Unity light.

## Light state and refilling

`SetLight` refuses an on request when health is not positive. It updates the `on` field, Unity light, paper material and particles. It also writes `ShipItem.amount` as `1` when on and `0` when off.

`OnLoad` uses an amount threshold of `1` to request the restored on or off state. `SetLight` still checks health before allowing the light to remain on.

`LoadFuel` distinguishes oil and candle fuel. Oil requests only the difference between current and initial health, then adds the supplied amount. Candle fuel resets health to `initialHealth`. The method rejects the wrong fuel type for the light.
