# Contributing to the Sailwind public reference

Contributions should make a bounded Sailwind finding easier to understand and verify. An article should identify the subject, explain the technical behavior directly and retain the relationships or qualifications that matter to a reader.

## Choose the owning repository

Put Sailwind systems, game-specific behavior and Sailwind mod cases in this repository. Put reusable Unity, BepInEx, Harmony, packaging and inspection methods in [Unity modding knowledge](https://github.com/Skeptic043/unity-modding-knowledge). Link the general method to the Sailwind case instead of maintaining two versions of the same explanation.

Extend an existing canonical article when new evidence answers the same question. Add a new page when it answers a distinct durable question or when one existing page combines independently useful systems that need separate ownership. Merge a very short page when it does not stand on its own, and remove a page from the reader-facing selection when it is redundant or mismatched. Preserve useful dated investigations as source records rather than deleting them or turning them into competing current explanations.

Subject pages are navigation routes, not exclusive ownership boundaries. Link one canonical article from each relevant route instead of copying its explanation into several categories.

## Use evidence where it matters

Distinguish the kind of evidence behind a claim when that distinction changes what the reader can conclude. Relevant evidence can include:

- official documentation or public source
- static managed-code or asset inspection
- a synthetic or managed fixture
- a Unity editor run
- live Sailwind gameplay
- a dated player report
- save and reload, multiplayer, performance or long-session testing

An earlier layer does not prove a later one. Static reachability does not prove live behavior. A clean loader entry does not prove that a feature was exercised. A player report from one build does not establish another build or platform. Keep these qualifications beside the affected claim rather than adding a routine evidence disclaimer to every article.

Give the game, loader, mod or dependency version when it changes how the reader should interpret the result, such as mixed-version evidence or a dependency-specific case. A coherent article about one current code path does not need a mandatory version section or a list of live behaviors that were not retested. Exact build IDs, hashes, module identities, tool versions and reproduction conditions normally belong in the private canonical source record. Include them in the reader article only when that exact identity is part of the technical finding.

## Link sources and protect private inputs

Link stable public source revisions, releases and documentation when they help the reader understand or verify the claim. Do not invent an attribution link from a name. If no stable public link was preserved, retain the exact identity in the private source record rather than turning provenance into reader-facing prose.

Do not commit or publish proprietary game assemblies, decompiled game source, original saves, personal logs, credentials, account names, machine-local paths or other private data. Inspection tools and compact original measurements are welcome when their inputs must be supplied independently. Review logs and generated outputs before sharing them.

## Article voice

Most articles should lead with the technical answer, then explain the mechanism, important relationships and meaningful edge cases. Retain useful class names, methods, fields, data relationships and verified constants when they help a technical reader understand the system. Add version, evidence or limits sections only when they materially affect interpretation. Do not use them as mandatory article furniture or repeat the same uncertainty at both the beginning and end. Recommendations belong where they follow from the evidence, but a bounded finding should not be presented as a universal rule. Procedural language remains appropriate when the subject is a tool, reproduction, diagnostic workflow or other method that readers must follow.

Keep investigation chronology, hashes, exact commands, inspector record counts, machine-local layout, failed attempts and detailed reproduction machinery in the supporting source record unless one of those details is the subject of the article. Keep unreleased product architecture, persistence schemas, implementation sequences and acceptance plans private when the public article only needs to explain the native Sailwind system.

Use concrete Sailwind names before introducing a local abstraction. Define terms such as operation identity in plain language on first use. Keep evidence limits next to the claims they qualify.

Use relative links between files in the same public repository so navigation stays on the branch, tag or commit being viewed. Remapped root files such as `README.md` and `CONTRIBUTING.md` may need navigation-only path adjustments in the public copy. Use stable absolute links across repositories. Avoid semicolons in public prose. Do not turn private plans, task records, test logs or implementation history into public reference material.

## Respect licenses and attribution

Only contribute material you are allowed to share. Attribute third-party code, documentation and assets under their applicable licenses. Do not copy third-party code or assets merely because they were inspected. This repository's original code uses MIT and its original written explanations use CC BY 4.0. See the [license scope](LICENSE.md).

## Review before publication

Review the exact diff, links, evidence identity, license treatment and tracked-file inventory. Check that a public article does not expose private paths, logs, credentials, proprietary inputs or internal workflow records. Re-run relevant reproduction tools when their code changes. Treat live gameplay, save and reload, multiplayer, performance and long-session acceptance as separate gates when the claim depends on them.
