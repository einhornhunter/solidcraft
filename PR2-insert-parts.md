Title: 3D printing: the Heat-Set Insert tool can place the brass inserts as parts, so the bill of materials counts them

## Change

This PR builds on PR1_LINK (the Heat-Set Insert tool, Refs #12378). The two PRs belong together, and this one can only be merged after that one. Until then, the diff here also shows that PR's commit.

The Heat-Set Insert tool drills holes for heat-set threaded inserts in 3D-printed parts. With this PR, a new option in its dialog, **Place Insert Parts**, also puts the brass insert itself into each hole:

- The insert is a brass sleeve of the chosen size, built from the same size table as the holes.
- The sleeve is one component, placed once in each hole and held there by a rigid joint. The bill of materials lists it once, with the number of holes as its count, for example "Heat-set insert M3x5.7, 2".
- The option is off by default.
- The feature manages its parts. When you edit the feature, for example to change M3 to M4 or to add a hole, the feature replaces its parts to match. When you turn the option off, the parts go away.
- The History still shows one entry, for example "HeatInsert1". The sketch and the revolve that build the sleeve stay out of the History, and so do the joints that hold it in the holes. "Show hidden" in the History's menu lists them again. This uses the component flag that Hide Component History already sets.
- A live preview shows only the holes, not the parts.

The command `plastic.heat_insert` takes `part: true` for the same result. The command `plastic.heat_insert_parts` makes the parts of one feature match it after an edit. The dialog sends it after `timeline.redefine`. `parts.insert` has a new `heat_set_insert` family.

**Design format.** The feature's `insert` saves two more fields: `part`, and `component` (the component of the parts, which the feature replaces). The change is one more file in `crates/doc/src/format/pending/`. Older designs have neither field and place no parts.

Changes a default: no

## Validation

- `cargo xtask ci`: RESULT
- `cargo test -p solidcraft-engine --lib`: RESULT
- New engine tests check four things. Two inserts give one bill-of-materials row with count 2. Each sleeve has the material Brass and the volume π × (2.3² − 1.5²) × 5.7 mm³. An edit from M3 to M4 replaces the parts. Turning the option off removes them.
- New UI test: with the parts on, the History shows only "Block" and "HeatInsert1" and no joints, in both History layouts. "Show hidden" brings the joints back.
- In the app at 1280×720: the flow below.

## Screenshots of the behavior

SCREENSHOTS

## Integration status

Stacked on PR1_LINK, rebased on upstream `main` at UPSTREAM_COMMIT. No conflicts are open. #16985 (thread options for Insert Part) also changes `crates/engine/src/cmd/parts.rs`. I will rebase on whichever PR lands first.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
