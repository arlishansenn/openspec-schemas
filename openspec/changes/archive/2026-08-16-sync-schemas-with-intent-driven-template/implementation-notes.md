# Implementation Notes

## Reference checkouts

Every file copied by this change traces to one of these commits:

| Repository | Commit | Date | Used for |
|---|---|---|---|
| [intent-driven-template](https://github.com/intent-driven-dev/intent-driven-template) | `57378eb90418a65c40c3ab3c40fcd40dd38f256f` | 2026-08-16 | `openspec/schemas/intent-driven/{schema.yaml,README.md,templates/*}` |
| [intent-driven-dev/skills](https://github.com/intent-driven-dev/skills) | `053dfff41b572714f868a34ce36248c86df099d9` | 2026-08-16 | Resolving `skills.txt` names; `spec-as-source` present, `bdd-zone-check` removed |

## Pre-copy verification (task 1.2)

Confirmed against the template checkout before copying:

- `openspec/schemas/intent-driven/schema.yaml` contains no `format:` block.
- `templates/design.md` and `templates/adr.md` are byte-identical to this repository's copies, so both are left unmodified.
- The artifact dependency graph is `proposal -> (specs, design)`, `adr -> design`, `tasks -> (specs, adr)` — identical to this repository's before the change.

## Deliberate deviations from the template

Per design decisions 1 and 2, `openspec/schemas/intent-driven/` is not a byte-for-byte copy. Three differences are intentional:

1. **`skills.txt` is retained.** The template has no such file; it is this repository's packaging contract, asserted by `custom-schema-packaging` and consumed by `AGENT_INSTALL.md` Step 6.
2. **The README keeps an "Associated Skills" section**, for the same reason.
3. **The README states the artifact flow as `proposal -> (specs, design) -> adr -> tasks`.** The template's README states a linear `proposal -> specs -> design -> adr -> tasks`, which contradicts its own `schema.yaml` (`specs` and `design` both declare `requires: [proposal]`). The linear line is a defect in the template and is worth reporting upstream.

`behaviour-driven` has no counterpart in the template and was realigned by analogy, per design decision 3.

## Archive-time follow-up

OpenSpec delta merge rewrites requirements, not the `## Purpose` prose at the top of a main spec. Two Purpose sections still describe the pre-change workflow and must be edited by hand when this change is archived:

- `openspec/specs/intent-driven-schema-workflow/spec.md` — "executable fenced-Gherkin behaviour specs and acceptance-test enforcement"
- `openspec/specs/behaviour-driven-schema-workflow/spec.md` — "extracted at test time, and run as the acceptance suite that every change must keep green"

Both should be restated so the executable-acceptance workflow is attributed to the `spec-as-source` skill rather than to the schema, matching the requirement deltas in this change. Every *requirement*-level reference to fenced Gherkin, the `format:` block, and the `stack:` key is already covered by the `MODIFIED` and `REMOVED` operations in `specs/`.
