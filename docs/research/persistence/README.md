# Persistence

Articles about native save capture and durable item identity.

- [Save capture and coroutine boundaries](save-capture-and-coroutine-boundaries.md) explains when Sailwind admits a save, where its save coroutine yields and when native and extension data are captured.
- [Item identity and restoration](item-identity-and-migration.md) separates template identity from per-item identity and follows both live and cached items through native restoration.

Charts are a concrete per-item data case. [Maps, Charts, and Editable Paper](../items/maps-charts-and-editable-paper.md) explains native `ChartData`, `SavePrefabData.chartData` and the separate extension-data dictionary.

Purchase-time ownership has its own ordering. See [how shop purchases establish item ownership](../shops/native-purchases.md) rather than treating `sold`, registration and payment as one event.

[Sailwind reference](../../../README.md)
