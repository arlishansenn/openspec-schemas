## 1. Prepare the reference checkout

- [x] 1.1 Clone `https://github.com/intent-driven-dev/intent-driven-template` to a scratch directory and record the commit SHA in the implementation notes so the source of every copied file is traceable
- [x] 1.2 Confirm the reference checkout still matches expectations: `openspec/schemas/intent-driven/schema.yaml` has no `format:` block, `templates/design.md` and `templates/adr.md` are identical to this repository's, and the artifact `requires:` graph is `proposal -> (specs, design)`, `adr -> design`, `tasks -> (specs, adr)`
- [x] 1.3 Clone `https://github.com/intent-driven-dev/skills` and confirm `.agents/skills/spec-as-source/` exists and `.agents/skills/bdd-zone-check/` does not, so the `skills.txt` updates in tasks 2.4 and 3.4 resolve

## 2. Realign the intent-driven schema

- [x] 2.1 Copy the reference `openspec/schemas/intent-driven/schema.yaml` over this repository's, replacing the fenced-Gherkin `specs` instruction with the OpenSpec Markdown delta instruction, widening `generates` to `specs/**/*.md`, replacing the stack-driven `tasks` instruction with the generic one, and removing the `format:` block entirely
- [x] 2.2 Copy the reference `templates/proposal.md`, `templates/spec.md`, and `templates/tasks.md` over this repository's copies
- [x] 2.3 Verify `templates/design.md` and `templates/adr.md` are byte-identical to the reference and leave them unmodified
- [x] 2.4 Rewrite `openspec/schemas/intent-driven/skills.txt` to: `acceptance-test-authoring`, `architectural-decision-records`, `c4-diagrams`, `gherkin-authoring`, `glossary`, `grill-me`, `openspec-git-discipline`, `spec-as-source` — one per line, no other content
- [x] 2.5 Rewrite `openspec/schemas/intent-driven/README.md` from the reference README, documenting the OpenSpec Markdown delta spec format, activation without a `stack:` key, and when `behaviour-driven` is the better fit
- [x] 2.6 In that README, state the artifact flow as `proposal -> (specs, design) -> adr -> tasks` rather than the reference's linear line, which contradicts its own `schema.yaml` (design decision 2)
- [x] 2.7 Add an "Associated Skills" section to that README listing exactly the eight skills from task 2.4 with a one-line purpose each, linking to https://github.com/intent-driven-dev/skills and noting installation into `.agents/skills/`
- [x] 2.8 In that README, attribute fenced-Gherkin authoring, acceptance-test execution, and specs/code zone isolation to the `spec-as-source` skill, and remove every claim that the schema itself enforces them
- [x] 2.9 Run `openspec schema validate intent-driven` and confirm it passes

## 3. Realign the behaviour-driven schema

- [x] 3.1 Update `openspec/schemas/behaviour-driven/schema.yaml`: replace the fenced-Gherkin `specs` instruction with the same OpenSpec Markdown delta instruction used in task 2.1, widen `generates` to `specs/**/*.md`, and remove the `format:` block
- [x] 3.2 Replace the stack-driven `tasks` instruction and `templates/tasks.md` with the generic numbered task-group form, removing every `stack:`, acceptance-suite, and red-green-commit reference
- [x] 3.3 Update `templates/spec.md` and `templates/proposal.md` to match the intent-driven versions from task 2.2, adjusted only where wording names the schema
- [x] 3.4 Rewrite `openspec/schemas/behaviour-driven/skills.txt` to: `acceptance-test-authoring`, `gherkin-authoring`, `glossary`, `openspec-git-discipline`, `spec-as-source` — one per line, no other content
- [x] 3.5 Rewrite `openspec/schemas/behaviour-driven/README.md`: document the OpenSpec Markdown delta format, the flow `proposal -> (specs, design) -> tasks`, activation without a `stack:` key, and that `intent-driven` is the same workflow plus durable ADRs
- [x] 3.6 In that README, attribute fenced Gherkin, acceptance testing, and the two spec-first rules to the `spec-as-source` skill, and remove every reference to the retired `bdd-zone-check` skill and to the `behavior-driven` American-spelling note tied to the old skill docs
- [x] 3.7 Add an "Associated Skills" section listing exactly the five skills from task 3.4 with a one-line purpose each
- [x] 3.8 Run `openspec schema validate behaviour-driven` and confirm it passes

## 4. Update root documentation

- [x] 4.1 Update the "Choosing a Schema" table in the root `README.md` so the `behaviour-driven` and `intent-driven` rows describe OpenSpec Markdown delta specs rather than fenced Gherkin and cucumber-js/behave
- [x] 4.2 Update the paragraph following that table so the `intent-driven` = `behaviour-driven` + durable ADR relationship is stated without claiming schema-level acceptance enforcement
- [x] 4.3 Rewrite the `Behaviour-Driven` and `Intent-Driven` catalog entries to attribute acceptance testing and fenced Gherkin to the `spec-as-source` skill, remove `stack:` from the activation snippets, and keep description, artifact order, activation, validate command, and schema README link
- [x] 4.4 Update the example `intent-driven` `config.yaml` in the root README to drop the `stack:` key
- [x] 4.5 Confirm the root README still links https://github.com/intent-driven-dev/intent-driven-template as the starter project and https://github.com/intent-driven-dev/skills for skills

## 5. Verify

- [x] 5.1 Run `openspec schema validate intent-driven` and confirm it passes
- [x] 5.2 Run `openspec schema validate behaviour-driven` and confirm it passes
- [x] 5.3 Run `openspec schema validate` for `spec-driven-with-adr`, `event-driven`, and `minimalist` as a regression check and confirm all pass
- [x] 5.4 Run `openspec validate sync-schemas-with-intent-driven-template --type change --strict` and confirm it passes
- [x] 5.5 Diff `openspec/schemas/intent-driven/` against the reference checkout and confirm the only differences are `skills.txt`, the "Associated Skills" section, and the corrected artifact-flow line from task 2.6
- [x] 5.6 Confirm every name in `openspec/schemas/*/skills.txt` resolves to a directory under `.agents/skills/` in the skills clone from task 1.3
- [x] 5.7 Grep the repository for `bdd-zone-check` and confirm no remaining reference outside `openspec/changes/archive/`
- [x] 5.8 Confirm `.claude/`, `.codex/`, and `.opencode/` are untouched by this change (`git status`)
