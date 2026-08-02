# Design: Adopt Fenced-Gherkin Behaviour-Driven Schema and Upgrade Intent-Driven

## Context

The source of truth is `/Users/harikrishnan/polarizer/projects/sdd/openspec/bdd/behavior-driven-template/openspec/schemas/behavior-driven/` (`schema.yaml` + `templates/{proposal,spec,design,tasks}.md`), a validated rewrite (openspec CLI 1.6.0 accepts its `format:` block). Its README/CLAUDE.md carry the workflow narrative ("two rules", supported stacks). The companion skills `acceptance-test-authoring` and `bdd-zone-check` are already published in intent-driven-dev/skills.

This repo's `behaviour-driven` schema still describes the old design (Markdown-wrapper requirements, root `features/*.feature` extraction, Cucumber.js-only gates, attempt limits), and `intent-driven` uses the wrapper format with an explicit no-`.feature`-files stance.

## Goals / Non-Goals

**Goals:**
- `behaviour-driven` becomes an exact functional port of the source schema, under the British-spelled name.
- `intent-driven` becomes "behaviour-driven + ADR": same spec format and acceptance enforcement, ADR artifact retained.
- Root README and both schema READMEs describe the new positioning accurately.
- Both skills.txt manifests reference the two new skills.

**Non-Goals:**
- No changes to `minimalist`, `event-driven`, `spec-driven-with-adr` schemas.
- No renaming to American spelling; no edits to the skills repo or the external `intent-driven-template` starter.
- No changes to `AGENT_INSTALL.md` flow beyond a one-line stack-key note.

## Decisions

- **Keep `behaviour-driven` naming; adapt content.** Only `name:`, description, and prose are britishized. The `format:` regexes, `# @openspec:` marker syntax, and quoted Gherkin examples stay byte-identical — tooling (extractors, effective-spec composers) matches on them. Both schema READMEs carry an explicit note that upstream skill docs show `schema: behavior-driven` but this package uses `behaviour-driven`. Alternative (rename to match upstream) rejected: breaks existing consumers of this repo.
- **intent-driven artifact graph unchanged:** `proposal [] → specs [proposal], design [proposal] → adr [design] → tasks [specs, adr] → apply [tasks]`; flow rendered everywhere as `proposal -> (specs, design) -> adr -> tasks`. Design is transitively required via adr, so `tasks.requires` stays `[specs, adr]`. Alternative (`tasks: [specs, design, adr]`) rejected: redundant edge, diverges from current file for no benefit.
- **intent-driven merges, not copies.** specs instruction and `format:` block are taken wholesale from the source; proposal keeps its ADR-distillation paragraph; design keeps its "read in-force ADRs first" preamble; tasks takes the source instruction plus an appended ADR-honouring sentence; tasks template takes the source 3-section structure with one added in-force-ADR line in section 2. Alternative (make intent-driven textually identical to behaviour-driven + adr artifact) rejected: would lose the ADR-aware guidance that is the schema's reason to exist.
- **`generates:` narrows to `"specs/**/spec.md"`** in both schemas (from `specs/**/*.md`): the extraction tooling anchors on `spec.md` filenames.
- **Old gate machinery is dropped, not ported**: feature-extraction/failing-acceptance/attempt-limit gates and the 30-line apply instruction are superseded by the acceptance-test-authoring contract (first-time setup, red→green→commit per pending step, completion). The old tasks template's self-referential `openspec/schemas/behaviour-driven/` validation task must not reappear.

## Risks / Trade-offs

- [Consumers on the old formats break] → Both changes are flagged **BREAKING** in the proposal; READMEs document the new format and the required `stack:` config key.
- [Spelling drift between package and upstream skill docs] → explicit spelling note in both READMEs; verification grep allows `behavior-driven` only in those notes.
- [British/American normalization corrupting a regex or marker] → verification asserts the two `format:` blocks are byte-identical to each other and YAML-parses both files; greps confirm marker syntax intact.
- [Repo's own specs drift from reality] → delta specs for the three affected capabilities are part of this change and are synced at archive time.

## Migration Plan

Single commit-series on main via the OpenSpec workflow (propose → apply → archive). Rollback = git revert. Downstream consumers migrate by rewriting specs into fenced-Gherkin form and adding `stack:` to `openspec/config.yaml`; the schema READMEs describe the target format.

## Open Questions

None.
