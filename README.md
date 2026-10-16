# Sailwind technical reference

Browse by game concept, then follow the focused articles into related systems. These subject pages are navigation routes rather than ownership boundaries, so one canonical article can appear wherever readers are likely to look for it.

## Sailwind systems

- [Items](docs/research/items/README.md): Item lifecycle, physical behavior, inventory, consumables, maps and equipment.
- [Boats](docs/research/boats/README.md): Boat identity, ownership state, runtime discovery and links into load, placement and persistence.
- [Physics](docs/research/physics/README.md): Mass, load boundaries, buoyancy, water interaction and physical placement.
- [World](docs/research/world/README.md): Ports, berths, placement, weather, time and player simulation.
- [Economy](docs/research/economy/README.md): Shops, purchases, physical stock, currencies and regional prices.
- [Persistence](docs/research/persistence/README.md): Save capture, durable item identity, migration and per-item data.
- [Interaction](docs/research/interaction/README.md): Input, UI, focused cameras, inventory presentation and audio.

## Modding and diagnostics around Sailwind

- [Mod discovery](docs/research/mod-discovery/README.md): Provider-aware discovery of mod-added equipment.
- [Profiles](docs/research/profiles/README.md): Sailwind profile, player and manager log origins.
- [Compatibility](docs/research/compatibility/README.md): The Radio and NLayer package case, with a link to the general Unity validation method.

## Community resources

These external community resources complement this repository. Check each project's documentation and releases for its current scope and compatibility.

- [Sailwind Discord](https://discord.gg/u4C8RQG6X9): Official community server for Sailwind news, discussion, help and modding.
- [Sailwind Wiki](https://sailwind.fandom.com/wiki/Sailwind_Wiki): Community-maintained reference for game mechanics, items, locations, navigation and other gameplay topics.
- [Sailwind Modding Directory](https://docs.google.com/spreadsheets/d/e/2PACX-1vQODnHApdvlj1wjJuofQpJVFt4Ow41-vwHublIwT81nnvCsASavFvJrBsYDT1iEjaaPBxUA0CLAaUOb/pubhtml#gid=0): Community-maintained directory of Sailwind mods organized by category, with links to their GitHub repositories or Thunderstore pages.
- [Sailwind Interactive Map](https://moffkalast.github.io/Sailwind-Map/): Interactive world map with route plotting, points of interest, trade winds, coordinates and other navigation tools.
- [Sailwind Mod Synchronizer](https://github.com/foxyv/SailwindModSynchronizer/releases/latest): Sailwind-specific mod manager, updater, catalog and modpack or profile launcher.
- [SAIL](https://github.com/SailwindModdingCommunity/SAIL): Community framework and API for adding ships and items to Sailwind.
- [Sailwind Modding Notes](https://github.com/ai-pop/sailwind-modding-notes): Collection of Sailwind reverse-engineering and technical research notes, with English translations of the original Russian notes.
- [Custom Island API](https://github.com/winterspices/CustomIslandAPI): API for mods that add custom islands and scenery.
- [Coordinate Viewer](https://github.com/AppSailwindMods/CoordinateViewer): Utility for viewing and exporting Sailwind world coordinates for use with the interactive map.

The articles lead with the technical answer and retain the classes, methods and data relationships needed to understand it. Version or evidence notes appear where they materially change how a claim should be read. Detailed investigations, tools, hashes, chronology and project-specific design normally remain in the private canonical research records unless one of those details is the article's subject.

For reusable engine and loader findings, see [Unity modding reference](https://github.com/Skeptic043/unity-modding-reference).

To propose a change, see [Contributing to the public reference](CONTRIBUTING.md).

## Licensing

Original written explanations use CC BY 4.0. Facts and measurements are outside that copyright claim. See the [license scope](LICENSE.md).

AI assisted the writing and organization of this reference.
