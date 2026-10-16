# Save capture and coroutine boundaries

A save occupies one interval that begins before the coroutine's first suspension and ends after the write path completes. Capture is not spread across several coroutine frames. The coroutine yields once before it creates the save container, then captures and writes the save in the resumed step without another yield.

[Persistence](README.md)

## The native sequence

`SaveLoadManager.SaveGame(bool)` admits a save only when the manager is not busy, the player is not in bed, `SaveLoadManager.readyToSave` is true and no shipyard is active. The Boolean argument selects compression. It does not identify whether the request was manual or automatic.

The admitted call starts `SaveLoadManager.DoSaveGame(bool)`. Its sequence is:

1. Set `SaveLoadManager.busy` to true.
2. Close the crate inventory UI, which can return its temporarily held item.
3. Yield one `WaitForEndOfFrame`.
4. Create `SaveContainer` and gather global, player, world, boat, item, mission and economy data.
5. Call `SaveLoadManager.SaveModData` after native prefab data has been gathered.
6. Rotate backups, create the output stream, serialize the container and close the stream.
7. Request cleanup, clear `busy` on the normal completion path and finish.

`SaveLoadManager.Update` contains three calls to `SaveGame(bool)`. The Boolean identifies compression, not request origin.

## Where extensions fit

Native item capture happens before `SaveModData`. That callback can write extension records into the same save container, but it cannot change native item data that has already been copied into the snapshot. Sailwind does not guarantee an update tick between a purchase, a cache transition and the next admitted save.

## Capture boundary

The single-yield layout means vanilla capture is not divided by repeated coroutine yields. Synchronous capture helpers and runtime patches can still do work during the resumed step.
