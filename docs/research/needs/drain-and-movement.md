# Needs drain and movement

Sailwind's passive drain and movement drain are separate. `PlayerNeeds.LateUpdate` invokes `DrainEnergyFromMovement`, then applies the ordinary food, water and awake sleep costs.

## Passive drain follows game time

`LateUpdate` subtracts these values while the player is eligible for needs updates:

- food: `Time.deltaTime * Sun.sun.timescale * 3`
- water: `Time.deltaTime * Sun.sun.timescale * 4`
- awake sleep: `Time.deltaTime * Sun.sun.timescale * 5`

These are code-path factors, not percentages per real-world second. Both frame delta and Sailwind's own time scale participate. Pausing, sleep, recovery, shipyard state and other surrounding branches can change whether this part of the method runs.

## Movement drain follows horizontal movement

`DrainEnergyFromMovement` selects costs before applying movement magnitude:

- Swimming while `OVRPlayerController.IsRunning()` is true selects `swimmingFoodCost` and `swimmingWaterCost`.
- Running on land selects `runningWaterCost` only.
- Other states leave both selected costs at zero.

The method reads `OVRPlayerController.outLastMovement`, sets its vertical component to zero and multiplies each selected cost by the resulting squared magnitude. Food below zero is charged to `foodDebt`. Water is subtracted directly. The method also asks `PlayerNeedsMinibar` to show the relevant needs when it selects a running or swimming cost.
