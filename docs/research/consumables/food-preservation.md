# Food preservation and slicing

Food preservation is not one progress value. Cooking, smoking, ambient drying, water exposure and spoilage use related but separate paths, while slicing and saving transfer specific parts of that state.

## Cooking and smoking

`ShipItemStove.AddHeat` sends `addedHeat * cookEfficiency` and the stove's `smoker` flag to every occupied `CookableFood` slot.

`CookableFood.Cook` calculates:

```text
increment = addedHeat * 0.066 / food.GetEnergyPerBite()
```

It adds `increment * currentHeat * 0.5` to `ShipItem.amount`, then adds `increment * 10` to `currentHeat`. Amount is capped at `2.2` and heat at `2`. When the smoker flag is true and a `FoodState` exists, it adds `increment * currentHeat * 0.5` to `smoked` after updating and clamping heat. `FoodState.AddSmoked` caps `smoked` at `1`.

The amount calculation uses the earlier heat value, while the smoking calculation uses the updated value.

## Drying, water and spoilage

For sold food, `FoodState.Update` begins ambient drying with:

```text
Time.deltaTime * Sun.sun.timescale / (30 * food.GetEnergyPerBite())
```

Salt multiplies that value by `Mathf.Lerp(1, 10, salted)`. A drying rack multiplies it by `2`. Layer `26` suppresses the ambient increment. Water replaces it with `-Time.deltaTime * 100 * Sun.sun.timescale`.

The method adds that result to `dried` and clamps it to the range 0 through 1. A smoking cook trigger then adds `CookableFood.GetCurrentHeat()` to `dried` after that clamp, so smoking can increase `dried` beyond the clamped ambient result.

The spoilage increment starts with `Time.deltaTime * Sun.sun.timescale / spoilDuration`. It is then multiplied by `Mathf.InverseLerp(1, 0, currentHeat)`, `1 - dried` and `1 - smoked`. A negative result is replaced with zero before it is added to `spoiled`.

## Slices and saved state

`ShipItemKnife.CutFood` instantiates `slicePrefabIndex` for `slicesCount`. Each slice receives the original item's amount plus `smoked`, `salted`, `dried` and `spoiled`, then registers with `SaveablePrefab`. The original item is destroyed after the slices are created.

`SaveablePrefab.PrepareSaveData` maps `dried`, `smoked`, `salted` and `spoiled` to `extraValue0` through `extraValue3`, and writes item amount to `itemAmount`. `Load` restores the same fields. `CookableFood.currentHeat` is not part of this food save mapping.
