# Native UI assets and resource ownership

Sailwind's native meshes, materials and fonts can provide a consistent visual style, but a borrowed scene reference and a resource created by a mod do not have the same owner or lifetime. Treating them separately avoids destroying native objects, retaining dead scene references or rebuilding valid owned resources unnecessarily.

## Native objects that carry the visual style

`StartMenu.chooseIslandUI` resolves the starting-island panel in the current scene. Its `bg` child uses the `notification_UI` mesh and the `notification_logo` material. The material uses `Legacy Shaders/VertexLit` and a 1024 by 1024 atlas texture.

The mesh reports `m_IsReadable=False`, `m_KeepVertices=False` and `m_KeepIndices=False`. Code that depends on runtime `Mesh.vertices` or other CPU mesh data is therefore relying on access the serialized asset does not promise. Reusing the native mesh through the rendering path, or providing separately owned geometry, respects that boundary.

The panel's `button confirm/text` child is a `TextMesh` with bold style, font size `60` and a native font reference. Borrowing that reference avoids distributing another font, but the reference still belongs to the scene object and can become stale when the scene is replaced.

## Borrowed references and owned caches

Borrowed references should be reacquired when their owning scene changes. A render texture, material instance or other resource created by the mod remains the mod's responsibility and can stay valid even after a donor reference expires. Cleanup should release owned resources without destroying native assets.

This distinction also matters during recovery. A lost scene reference does not by itself invalidate an owned cache, while a missing or lost owned render target needs recreation even if the borrowed font and style references remain valid.

## Destination scale and source borders

Nine-slice borders describe coordinates in the source texture. If a 12 by 12 texture has six-pixel border insets, multiplying those source insets when the destination is enlarged makes the borders exceed the source dimension. Keep the source border values fixed while scaling the destination rectangle, font and padding.

Source coordinates and destination scale answer different questions. Scaling the destination changes how large the result appears, while changing a source border changes which texels are treated as fixed edges.

## Geometric fit is not rendered readability

Text bounds and available inner width can establish that a label fits geometrically. They do not establish contrast, atlas sampling, font sharpness, localization, clipping at another resolution or readability against the rendered background.

The native hierarchy, references, shader, dimensions and readability flags describe the available assets. Geometric fit and rendered readability remain separate properties across resolutions, UI scales and languages.
