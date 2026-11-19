Title: 3D printing: drill holes for heat-set threaded inserts and check the wall around them (#12378)

## Change

This adds a **Heat-Set Insert** tool for 3D-printed parts. A heat-set insert is a brass sleeve with a thread inside. You melt it into a printed hole with a soldering iron, so the part can take a metal screw. The tool is in the Create group of the PLASTIC commands, next to Boss.

Refs #12378. This is the first of two PRs for that issue, and the two belong together. The second PR builds on this one: it places the brass inserts themselves as parts in the holes, so the bill of materials counts them. It can only be merged after this one.

What a user can do now:

- **Choose a size instead of typing numbers.** The dialog lists 17 sizes: M2 to M8 and the UNC sizes #2-56 to 3/8"-16. Each size gives the hole diameter, the insert's outer diameter and length, and the least wall it needs. The default depth is the insert length plus 1 mm, so the melted plastic has room below the insert.
- **Pick places in the model.** Click points on a flat face, as many as you want. A second click on a placed insert takes it out. Or click the points of a sketch. Inserts on sketch points follow the points: move a point in its sketch and the hole moves with it.
- **Get a wall and floor check.** Before the tool changes the design, it measures the wall around each insert and the floor below each hole. It measures on the body's exact geometry, not on the display mesh. A thin wall stops the tool with a message such as "the wall around the insert is 0.80 mm; this insert needs at least 1.6 mm". With "Thin Wall: Warn Only", the tool drills anyway and reports a warning. The feature checks again whenever it is rebuilt, for example after a sketch point moves too near an edge.
- **Add an optional lead-in chamfer.** A 45° chamfer at the mouth of the hole guides the insert in. By default it goes out to the insert's outer diameter. You can type another width. A chamfer wider than the insert counts in the wall check.
- **Set an optional depth.** Leave the field empty for the default, or type your own depth.
- **Edit it later as one feature.** The tool makes one feature, for example "HeatInsert1", with its own icon in the History. Edit Feature opens the Heat-Set Insert dialog again with the size, the places and the options. When you change the size, the holes change with it.

The command `plastic.heat_insert` takes every value: outer diameter, length, hole diameter, least wall, least floor, extra depth or depth, chamfer and check mode. The command `plastic.heat_insert_sizes` lists the built-in sizes.

**Where the sizes come from.** The values are the published dimensions from the ruthex RX series datasheet (metric and UNC pages, status 15 Aug 2022). The app and the code use neutral size names and no brand name. ATTRIBUTION names the source. The help text says that the values are starting points.

**Design format.** The Hole feature saves two more fields: `insert` (the size and the values the check needs) and `more` (more places on the same face). The change is one file in `crates/doc/src/format/pending/`. Older designs have neither field, so they need no step.

**Not in this PR.** The issue also asks for loading your own size table from a JSON file. That is left for a later PR.

Changes a default: no

## Validation

- `cargo xtask ci`: RESULT
- `cargo test -p solidcraft-engine --lib`: RESULT
- The Heat-Set Insert tests pass: 13 engine tests, the doc tests for the feature and the format, and 4 dialog tests. The engine tests compare the removed volume with a hand calculation. For example, two M4 holes after an edit remove 2 × π × 2.8² × 9.1 mm³.
- In the app at 1280×720: the full flow below, and a small-window check.

## Screenshots of the behavior

SCREENSHOTS

## Integration status

Rebased on upstream `main` at UPSTREAM_COMMIT. No conflicts are open.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
