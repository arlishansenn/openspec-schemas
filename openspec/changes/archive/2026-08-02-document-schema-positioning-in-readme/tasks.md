## 1. Choosing a Schema section

- [x] 1.1 Add a "Choosing a Schema" section to root `README.md` after the intro and before "Install a Schema", stating `spec-driven` is the upstream default for most projects and `intent-driven` is the most complete general-purpose schema for complex projects
- [x] 1.2 Add a comparison table (Schema | Artifact flow | Choose when) with rows for `spec-driven` (built-in), `minimalist`, `behaviour-driven`, `spec-driven-with-adr`, `intent-driven`, and `event-driven`, sourcing artifact flows from each `openspec/schemas/<name>/schema.yaml`
- [x] 1.3 Add relationship prose below the table: intent-driven subsumes spec-driven-with-adr; intent-driven adopts behaviour-driven's Gherkin style but not its executable acceptance-test enforcement (`.feature` files, Cucumber gates); event-driven is for event-centric/AsyncAPI-first systems; minimalist is for small, low-risk changes

## 2. Catalog restructure

- [x] 2.1 Change `## Spec-Driven With ADR` to `###` and move it under "Custom Schemas" before the Intent-Driven entry (order: Minimalist, Event-Driven, Behaviour-Driven, Spec-Driven With ADR, Intent-Driven)
- [x] 2.2 Normalize every catalog entry to the same shape: description, artifact order code block, Activation YAML, Validate command, link to the schema README (add missing pieces to Minimalist, Event-Driven, Behaviour-Driven, Spec-Driven With ADR)
- [x] 2.3 Add one-line cross-references in the Spec-Driven With ADR and Behaviour-Driven entries pointing at their relationship to Intent-Driven

## 3. Intent-driven starter template

- [x] 3.1 Link https://github.com/intent-driven-dev/intent-driven-template from the Intent-Driven catalog entry (and a one-liner in Choosing a Schema) as a starter project with the schema, config, commands, and companion skills pre-installed — linked as a template only, keeping skills references at https://github.com/intent-driven-dev/skills

## 4. Verification

- [x] 4.1 Confirm no files under `openspec/schemas/` changed and run `openspec schema validate <name>` for each packaged schema as a sanity check
- [x] 4.2 Review README rendering: headings nest correctly under "Custom Schemas", table renders, and template/skills links resolve
- [x] 4.3 Run `openspec validate --change document-schema-positioning-in-readme` (or equivalent) to confirm the delta spec parses
