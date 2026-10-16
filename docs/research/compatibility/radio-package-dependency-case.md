# Case study: Radio package dependency failure

An incomplete Radio handoff contained `SailwindRadio.dll` without its pinned MP3 decoder, NLayer 1.16.0. The Radio loaded far enough to attempt playback, but each MP3 failed and the queue advanced rapidly. Retrying the same Radio candidate with the complete bundle removed that missing-decoder loop.

This is a bounded packaging case, not a claim of general Radio or Sailwind compatibility. For the evidence levels used here, see [Compatibility evidence layers](https://github.com/Skeptic043/unity-modding-knowledge/blob/main/topics/validation/compatibility-evidence-layers.md).

## Failure path

The selected `Player.log` repeatedly reported failure to load `NLayer, Version=1.16.0.0` through Radio's MP3 decoder. The log established dependency-resolution failure, not the physical absence of NLayer from an identified profile.

Radio routes decoder failures into track failure and queue advancement. When every MP3 takes the same failure path, playback can advance repeatedly until it reaches another playable format or exhausts the queue.

## Complete-bundle retry

Radio's package stages `SailwindRadio.dll` and its pinned NLayer DLL together. The user retried the same Radio candidate with that complete bundle, and the rapid missing-decoder loop disappeared. The selected follow-up log contained no Radio decode or missing-NLayer warning.

The retry closes that missing-dependency problem for the candidate, NLayer 1.16.0 and bundle that were supplied. It does not explain unrelated skips or format failures.
