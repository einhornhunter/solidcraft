# Heat-Set Insert (#12378): handoff

Übergabe für den neuen Rechner / handoff for the next machine. Nothing is public yet: no PR is open
on tangency-labs/solidcraft. Ask the owner (Einhornhunter) before every public action.

## Branches (on origin = einhornhunter/solidcraft)

| Branch | What |
|---|---|
| `heat-set-insert-tool` | **PR 1**: the whole tool without brass parts (command, feature, dialog, sketch points, chamfer, depth, wall check). Based on upstream/main 517cf0145. |
| `heat-set-insert-parts` | **PR 2**, stacked on PR 1: brass insert parts, bill of materials, parts kept out of the History. |
| `heat-set-insert-all` | Everything in one (same final tree as PR 2), in case one PR is wanted after all. |
| `heat-set-insert-notes` | This note and the two PR texts (orphan branch, not for a PR). |
| `heat-set-insert-12378`, `heat-set-insert-part`, `heat-set-insert-dialog` | Old work branches on an older main; superseded. |

The `wip:` commits must be squashed before the PRs: per branch one commit with a real message,
author `Einhornhunter <hubertus.kuehn@gmail.com>`, plus the claim commit `claim: #12378` (empty)
first on PR 1, as `skills/solidcraft-contributing/SKILL.md` says.

## Still to do

1. `cargo xtask ci` on `heat-set-insert-tool`, then on `heat-set-insert-parts` (never switch branches while it runs).
   Also `cargo test --profile ci -p solidcraft-engine --lib` and the ui-egui heat_insert tests.
   Known failures unrelated to this work (fail on plain main too): model_cache `a_later_round…`,
   `each_format_has_an_export_command_of_its_own`, `a_hole_wider_than_the_part…`; SMLib fillet/shell/thicken,
   GPU snapshot and raytrace-cpu render tests and space-mouse timing are environmental.
2. Screenshots at 1280×720 plus a small-window check (app under Xvfb with `--control PORT`, see
   `skills/solidcraft-agent-tools/SKILL.md`): the dialog, picks on a face, picks on sketch points,
   a thin-wall refusal, the single History entry, Edit Feature, and for PR 2 the parts + BOM.
3. Fill RESULT / SCREENSHOTS / UPSTREAM_COMMIT / PR1_LINK in the PR texts.
4. Show the owner the final texts and screenshots; only after the owner's yes: push, open PR 1 as
   draft claim, then PR 2.

## Open questions for the maintainers (put in PR 1)

- The size values come from the ruthex RX datasheet, named only in ATTRIBUTION and the PR (asked in the issue).
- Loading a user size table from JSON (in the issue) is not done; a later PR.
- The icon `heat_insert` awaits the owner's acceptance (`requested_icons.txt`: false).
- Layering: the wall check lives in `crates/doc/src/heat_insert.rs` and uses kernel Booleans; new rule says
  SolidCraft computes no geometry and recipes belong in solidcore-ops: ask whether to move it.
