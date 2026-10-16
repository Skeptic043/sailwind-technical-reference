# Sleep and settings-menu time scale

Sailwind's native sleep sequence changes both `Time.timeScale` and `Time.fixedDeltaTime`. It also changes `GameState.sleeping` and `GameState.eyesFullyClosed`, so sleep is not represented solely by the numeric time scale.

[Time](README.md)

## Native sleep sequence

`Sleep.Awake` sets `Time.fixedDeltaTime` to `0.02222`, records that value as `initialTimeStep`, calculates `sleepTimeStep` as ten times the initial step and sets `sleepTimescale` to `16`.

`Sleep.FallAsleep` sets `GameState.sleeping` and starts `StartSleepTimeWarp`. The iterator first yields `WaitForSeconds(3)`, a three-scaled-second wait. In its resumed step it writes `sleepTimeStep` to `Time.fixedDeltaTime`, writes `sleepTimescale` to `Time.timeScale` and sets `GameState.eyesFullyClosed`.

`Sleep.WakeUp` requires `eyesFullyClosed` before it proceeds. It clears `GameState.sleeping` and `GameState.eyesFullyClosed`, later restores `initialTimeStep` to `Time.fixedDeltaTime` and writes `1` to `Time.timeScale`.

## Settings-menu sequence

`StartMenu.GameToSettings` reads the current `Time.timeScale` into `StartMenu.unpausedTimescale`, then writes `0` to `Time.timeScale`. `StartMenu.SettingsToGame` later restores the cached `unpausedTimescale`.

The menu therefore preserves the scale it observed when it opened, even when that value was not the normal scale of `1`. It does not cache or restore `Time.fixedDeltaTime`.

## Sleep state beyond the numeric values

The conjunction `GameState.sleeping && GameState.eyesFullyClosed` identifies the active native sleep warp. Before `eyesFullyClosed` becomes true, the sleep transition has begun but the coroutine has not yet made its time-value changes.

Other systems also read those flags. `OceanUpdaterCrest.DCTInertiaUpdate`, for example, uses `GameState.eyesFullyClosed` in its calculation. Matching sleep's time-scale value alone does not reproduce those state-dependent behaviors.
