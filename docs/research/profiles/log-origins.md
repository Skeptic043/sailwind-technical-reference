# Case study: profile and player log origins

A selected Sailwind mod profile's BepInEx log and Unity's `Player.log` come from different Windows locations and can describe different launches. Collecting both is useful, but their presence does not prove a matching session.

## Distinct log owners

In this Windows case:

- `instance/BepInEx/LogOutput.log` belonged to the selected profile or pack.
- `Player.log` belonged to Sailwind's shared Unity player-data directory.
- the manager log belonged to the launcher or profile manager.

A save-directory override changes save operations. It does not by itself redirect Unity's player log. Proton and other platforms can use different layouts, so recheck their platform helpers instead of copying Windows assumptions.

For the general method of matching timestamps, startup markers and profile identity before combining logs, see [Compatibility evidence layers](https://github.com/Skeptic043/unity-modding-knowledge/blob/main/topics/validation/compatibility-evidence-layers.md).

## Selected diagnostic exports

A selected diagnostic export can contain only the current files requested, such as the profile loader log, Unity player log and manager log, while preserving their bytes and timestamps. Configurations, saves, rotated logs and complete directory trees are separate data rather than implied parts of that set.

Logs can contain local paths, account names, media names and other private values. Review them locally before sharing. If a log is still being written, treat the collected files as a best-effort snapshot rather than a consistent transaction.
