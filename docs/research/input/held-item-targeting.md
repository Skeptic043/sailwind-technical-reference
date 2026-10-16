# Held-item targeting and action ownership

Target selection and target action are separate contracts. A held item decides whether a candidate button can become the pointer target, while the selected button still owns the click, occupancy checks and resulting action.

## Target admission happens first

`GoPointer.DoRaycast` reads `heldItem` and calls its virtual `AllowOnItemClick` method for the candidate `GoPointerButton`. A permitted candidate can then be assigned to `pointedAtButton`. A rejected candidate follows a different branch before pointer assignment.

The base `ShipItem.AllowOnItemClick` returns true only when the held item is not marked `big` and the candidate button has `allowPlacingItems`. It does not inspect `HangableItem` or lamp-hook compatibility.

`ShipItemHangable.AllowOnItemClick` uses a different rule. It returns true when the candidate has `ShipItemLampHook`.

This means adding `HangableItem` to an object gives it attachment behavior but does not automatically replace the targeting rule inherited from plain `ShipItem`. A custom item can be physically attachable and still never admit the hook as its pointer target.

## The target still owns the action

`ShipItemLampHook.OnItemClick` first rejects an occupied hook. For an available hook it obtains `HangableItem` from the clicked item and calls `HangableItem.ConnectJoint` when that component exists.

Admission does not perform the attachment. Calling `AllowOnItemClick` and calling `ConnectJoint` are different stages, with the hook retaining the occupancy check and action.

The ordinary release and throw paths are separate from target admission. A successful button target can suppress or redirect the normal held-item path, so the result of a drop input in the same frame also depends on those paths.
