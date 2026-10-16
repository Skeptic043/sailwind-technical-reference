# Maps, Charts, and Editable Paper

Sailwind's chart system combines a physical foldable item, a focused interaction camera, per-item drawing data and runtime texture rendering. Those systems make existing charts a plausible foundation for custom maps, blank paper, notes and other editable surfaces, while the native player-facing implementation is primarily built around two-point line drawing.

[Items](README.md)

## Native chart components

| Component | Role |
| --- | --- |
| `ShipItemFoldable` | Folding, unfolding, physical item state and charting eligibility |
| `MapTableCamera` | Focused chart view, pointer input, zoom and panning |
| `MapChart` | Chart interaction and drawing coordinates |
| `MapChartTextureRenderer` | Rendering drawing geometry onto the chart surface |
| `ChartData` | Per-chart line and point data |

`ShipItemFoldable` provides the physical chart behavior. Native regional charts enable `allowCharting`, reference a `MapChart` and support wall attachment. `ShipItemFoldable.OnItemClick` admits native charting only when the chart is unfolded, the held item is exactly a `ShipItemInkSet` and the chart is within 20 degrees of horizontal.

`MapTableCamera` handles the close-up chart view. It supports pointer interaction, drag panning and a 20–90 degree field-of-view range. Entering this mode hides the held kit renderer and disables normal player control and mouse look, restoring those states on exit. Its native activation path assumes a real held charting kit, so it is not a generic document-view camera as-is.

## Drawing and rendering

Native charting uses two-click line drawing. `MapChart.OnActivate` records line endpoints, while `MapChart.OnLook` updates and previews the current endpoint in chart-local coordinates. `ChartData` contains:

- `tempLine`
- `lines`
- `points`

Stored lines contain two endpoints and a color index. Points contain two coordinates. Although the data model includes points, the native interaction does not provide a complete player-facing point-placement tool, and the scene renderer's `pointPrefab` is null.

Drawing geometry and its visible representation are separate. `MapChart.Awake` creates an instance material and cloned render texture, while `MapChartTextureRenderer.SpawnObjects` uses pooled `LineRenderer` objects and a shared render camera to draw the stored geometry onto that texture. The persisted data therefore describes the drawing, while the render texture provides its visible presentation.

## Persistence

Chart data is saved per physical item. `SaveablePrefab.PrepareSaveData` stores chart information in `SavePrefabData.chartData`, and `SaveablePrefab.Load` restores it. Separate chart instances can therefore retain separate drawings. The native `ChartData` model contains lines and points, but no fields for arbitrary text, freehand strokes, editable labels or other richer annotation types.

Separately, Sailwind saves `GameState.modData` as a `Dictionary<string, string>` for extension-owned data. It is independent of `ChartData`. Additional per-document information would therefore need its own data model and a reliable association with the correct physical item.

## Physical paper and map assets

The game contains separate paper meshes and map materials that could serve as foundations for blank or custom documents. The native chart render texture is 2048 × 2048. Some map materials reference both `_MainTex` and `_EmissionMap`, so changing only the main texture can leave portions of the original printed map visible. Blanking an existing map changes its presentation, but does not by itself create a general-purpose document system.

There is community precedent for repurposing the native maps this way. [Scrambled Seas](https://github.com/NANDbrew/scrambled-seas/releases) v7.2.0 blanked the in-game maps for player charting, using modified textures credited to Lynsis. Its v7.2.1 release fixed a case where those maps did not blank correctly when loading certain saves. The release notes establish the intended behavior, not the mod's implementation or current compatibility.

## Native limits

The chart system provides:

- physical foldable charts
- focused chart interaction
- panning and zooming
- two-point line drawing
- persistent per-item chart data
- runtime rendering of those drawings

The native implementation does not provide player-facing support for:

- typed text
- editable labels
- freehand drawing
- selecting or moving existing annotations
- erasing individual annotations
- undo and redo

The existing chart architecture therefore provides several foundations for richer paper or annotation systems, but those editing features require additional behavior rather than simply exposing functionality that already exists.

## Related articles

- [Item identity and restoration](../persistence/item-identity-and-migration.md) explains native per-item identity and when extension data becomes available during load.
- [Held-item targeting and action ownership](../input/held-item-targeting.md) separates selection of an interaction target from the action it performs.
- [Native UI assets and resource ownership](../ui/native-assets-cache-and-labels.md) covers borrowed scene references, mod-owned rendering resources and the difference between geometric fit and rendered readability.
- [Interaction](../interaction/README.md) links the broader input, UI, camera and audio routes.
