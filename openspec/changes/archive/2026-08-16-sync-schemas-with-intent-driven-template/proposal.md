## Why

The `intent-driven-template` repository has moved the BDD machinery out of the schema and into an opt-in `spec-as-source` skill, leaving the schema itself vanilla OpenSpec. This repository's `intent-driven` and `behaviour-driven` schemas still embed that machinery (fenced-Gherkin `format:` block, `stack:` key, acceptance-enforcement rules), so the packaged schemas no longer match the reference project they point readers at.

## What Changes

- Realign `openspec/schemas/intent-driven/` with the template: replace the fenced-Gherkin `specs` instruction with OpenSpec Markdown delta headers (`## ADDED Requirements` / `### Requirement:` / `#### Scenario:`), widen `generates` to `specs/**/*.md`, remove the `format:` block, and revert `templates/spec.md`, `templates/tasks.md`, and `templates/proposal.md` to the template's versions. **BREAKING** for projects authoring fenced-Gherkin specs against this schema.
- Replace the schema's stack-driven `tasks` instruction with the template's generic one; drop `stack:` from activation guidance.
- Rewrite `openspec/schemas/intent-driven/README.md` to the template's, retaining this repository's "Associated Skills" section (the template has no `skills.txt` convention).
- Apply the same separation to `openspec/schemas/behaviour-driven/`, which shares the format block and stack-driven tasks that `intent-driven` is shedding.
- Update both `skills.txt` manifests: drop `bdd-zone-check` (deleted upstream), add `spec-as-source`.
- Update the root `README.md` so its schema positioning stops describing acceptance enforcement as a schema property.
- Leave `.claude/`, `.codex/`, and `.opencode/` assets untouched — this repository's copies are newer than the template's.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `intent-driven-schema-workflow`: spec format moves from fenced Gherkin with `# @openspec:` markers to OpenSpec Markdown delta headers; the `format:` block, `stack:` key, acceptance-enforcement rules, and stack-driven task template are removed from the schema and delegated to the `spec-as-source` skill.
- `behaviour-driven-schema-workflow`: same separation — schema keeps the proposal-to-tasks workflow, the BDD workflow becomes skill-owned.
- `custom-schema-packaging`: `skills.txt` contents for both schemas change, and the root README's inter-schema positioning is restated without the schema-embedded acceptance claims.

## Impact

- `openspec/schemas/intent-driven/` — `schema.yaml`, `README.md`, `skills.txt`, `templates/{proposal,spec,tasks}.md`
- `openspec/schemas/behaviour-driven/` — same file set
- `README.md` (root) — "Choosing a Schema" table and the `Behaviour-Driven` / `Intent-Driven` catalog entries
- Downstream: projects that installed the current schemas and authored fenced-Gherkin specs must either adopt the `spec-as-source` skill or pin the previous schema version
- Upstream dependency already satisfied: `intent-driven-dev/skills` now ships `spec-as-source` and has removed `bdd-zone-check`
- Verification: `openspec schema validate intent-driven` and `openspec schema validate behaviour-driven`
