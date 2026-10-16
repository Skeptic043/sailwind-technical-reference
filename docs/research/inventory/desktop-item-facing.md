# Desktop inventory facing

Inventory item rotation is relative to the desktop UI anchor and inventory slot, not just the item's serialized rotation fields. In the Sailwind 0.39.2 `level24` scene, a model whose front is local negative Z faces away from the desktop camera at zero adjustment. A local Y adjustment of 180 degrees faces it toward the camera while restoring camera-right.

## Native rotation path

`PlayerNeedsUI.UpdateAnchor` calls `OVRPlugin.GetSystemHeadsetType`. On desktop it reparents the UI to `PCParent`, then sets the UI object's local position to zero and local rotation to identity. VR selects `VRParent` instead.

`ItemRigidbody.LateUpdate` builds the inventory rotation target from:

```text
currentInventorySlot.rotation
* Quaternion.Euler(item.inventoryRotationX, item.inventoryRotation, 0)
```

`PlayerNeedsUI.ToggleInventory` changes inventory scale and item layers. It does not supply an additional item-facing rotation.

## Desktop scene basis

That scene contains five inventory slots whose final local rotations are identity. Combining the desktop anchor and inventory-parent rotations produced these directions for a model with local negative Z as its front:

| Local Y adjustment | Camera-local front | Camera-local right | Result |
| ---: | --- | --- | --- |
| 0 degrees | approximately `(0, -0.412, 0.911)` | approximately `(-1, 0, 0)` | front points away and right is mirrored |
| 180 degrees | approximately `(0, 0.412, -0.911)` | approximately `(1, 0, 0)` | front points toward the camera and right is restored |

Those directions describe one model-axis convention. A mesh authored with another forward or up axis needs its own adjustment.

These directions describe desktop transform-facing for this scene. Rendered readability and VR presentation are separate concerns.
