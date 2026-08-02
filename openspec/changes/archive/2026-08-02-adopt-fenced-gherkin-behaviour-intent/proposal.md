# Adopt Fenced-Gherkin Behaviour-Driven Schema and Upgrade Intent-Driven

## Why

The upstream behavior-driven-template repo has fully rewritten the behaviour-driven workflow: specs are now Markdown files with executable fenced ` ```gherkin ` blocks (extracted and run as the acceptance suite via cucumber-js or behave), replacing the old `### Requirement:`/`#### Scenario:` Markdown-wrapper format and its Cucumber.js-only gate contract. The schemas packaged here have diverged from that rewrite, and the two new supporting skills (`acceptance-test-authoring`, `bdd-zone-check`) already published in intent-driven-dev/skills are referenced by no schema. Syncing now keeps this repo the canonical distribution point for the current workflow.

## What Changes

- **BREAKING** — `behaviour-driven` schema is replaced wholesale: specs use column-0 ` ```gherkin ` fences (`Feature:` = capability, `Rule:` = requirement, `Scenario:` = example) with `# @openspec: ADDED|MODIFIED|REMOVED|RENAMED` delta markers, a machine-readable `format:` block in `schema.yaml`, a simplified `apply:` instruction, and a stack-agnostic (javascript/python) 3-section tasks template driven by `stack:` in `openspec/config.yaml`. The old root `features/*.feature` extraction, Cucumber.js-only gates, and attempt-limit loops are removed. Schema name keeps the British spelling `behaviour-driven`.
- **BREAKING** — `intent-driven` becomes "behaviour-driven + ADR": it adopts the same fenced-Gherkin spec format, `format:` block, acceptance-test enforcement, and stack-driven tasks template, while keeping its `adr` artifact, immutable-ADR rules, and ADR-aware design instruction. Its previous "no `.feature` files, no lint" stance is removed.
- `skills.txt` manifests gain `acceptance-test-authoring` and `bdd-zone-check` (behaviour-driven: 5 skills; intent-driven: 8 skills).
- Both schema READMEs are rewritten for the new format (two rules, supported-stacks table, spelling note that upstream skill docs say `behavior-driven` while this package uses `behaviour-driven`).
- Root `README.md` repositioning: intent-driven now shares behaviour-driven's executable acceptance enforcement (the "intentionally not its executable acceptance-test enforcement" claim is deleted); table rows, relationship paragraph, example config (`stack:` key), and per-schema sections updated.
- `AGENT_INSTALL.md` gains a one-line note that some schemas require extra `config.yaml` keys (`stack: javascript|python`).

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `behaviour-driven-schema-workflow`: near-full rewrite — fenced-Gherkin spec format with `# @openspec:` delta markers and `format:` block replaces the Markdown-wrapper format; feature-extraction/failing-acceptance/attempt-limit gate requirements are replaced by the acceptance-test-authoring contract (stack-agnostic first-time setup, red→green→commit per step, completion); skills manifest and README requirements updated.
- `intent-driven-schema-workflow`: spec format switches to fenced Gherkin with `format:` block; tasks/apply adopt the acceptance-test workflow; the "no `.feature` files / external linting out of scope" requirements are removed; ADR requirements unchanged; skills manifest and README requirements updated.
- `custom-schema-packaging`: declared skills lists for the two schemas change; the root-README positioning requirement asserting intent-driven lacks executable acceptance-test enforcement is replaced with "behaviour-driven + ADR" positioning.

## Impact

- `openspec/schemas/behaviour-driven/` — schema.yaml, all four templates, skills.txt, README.md (full replacement).
- `openspec/schemas/intent-driven/` — schema.yaml (specs/tasks/proposal instructions, `format:` block), spec.md + proposal.md + tasks.md templates, skills.txt, README.md; design.md/adr.md templates and adr artifact unchanged.
- Root `README.md`, `AGENT_INSTALL.md`.
- Consumers activating `schema: behaviour-driven` or `schema: intent-driven` must migrate specs to the fenced-Gherkin format and add `stack: javascript|python` to `openspec/config.yaml`; existing `### Requirement:`-style specs will no longer match the schema contract.
- External `intent-driven-template` starter repo will lag the new intent-driven schema (out of scope here).
- Depends on skills `acceptance-test-authoring` and `bdd-zone-check` already published in intent-driven-dev/skills (confirmed present).
