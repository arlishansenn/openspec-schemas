## Why

The root `README.md` lists the custom schemas but gives readers no way to choose between them: there is no guidance on when the upstream `spec-driven` default is enough versus when to reach for a richer schema, the relationships between schemas (intent-driven subsumes spec-driven-with-adr; behaviour-driven's executable BDD gates are deliberately not part of intent-driven) are undocumented, and the catalog itself is structurally inconsistent (`## Spec-Driven With ADR` sits outside the "Custom Schemas" section, and only some schemas get activation/validate snippets). Readers also have no pointer to the fastest way to try the flagship schema: the `intent-driven-template` starter repo with the schema pre-installed.

## What Changes

- Add a "Choosing a Schema" section to the root `README.md` that positions `spec-driven` as the default for most projects and `intent-driven` as the most complete general-purpose schema for complex projects, with a compact comparison table (schema, artifact flow, choose-when) covering all packaged schemas plus the upstream `spec-driven` built-in.
- Document the schema relationships accurately: `intent-driven` shares the artifact graph of `spec-driven-with-adr` and adds Gherkin-style specs plus a larger skill set; `behaviour-driven` remains the choice for executable BDD (`.feature` files, Cucumber acceptance gates) which `intent-driven` intentionally excludes; `event-driven` is domain-specific for event-centric/AsyncAPI-first systems; `minimalist` is for small, low-risk changes.
- Link https://github.com/intent-driven-dev/intent-driven-template from the intent-driven catalog entry as a starter project with the schema, config, commands, and companion skills already installed (linked as a template only, not as a skills source — skills remain canonical at intent-driven-dev/skills).
- Fix the `## Spec-Driven With ADR` heading level so it nests under "Custom Schemas", and normalize every schema entry to the same shape: description, artifact order, activation YAML, validate command, and link to the schema README.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `custom-schema-packaging`: The root-catalog requirement gains schema-selection guidance — the root `README.md` SHALL help readers choose a schema (default vs most-complete positioning, comparison of packaged schemas, documented inter-schema relationships), SHALL present every catalog entry with a consistent structure nested under one catalog section, and SHALL reference the `intent-driven-template` starter for trying the intent-driven schema.

## Impact

- `README.md` (repository root) — only file changed at implementation time.
- No files under `openspec/schemas/` change; `openspec schema validate` for each packaged schema should continue to pass as a sanity check.
