# Tasks

Source of truth for imported content: `/Users/harikrishnan/polarizer/projects/sdd/openspec/bdd/behavior-driven-template/openspec/schemas/behavior-driven/` (schema.yaml + templates), with its README.md/CLAUDE.md informing README rewrites. Never britishize the `format:` regexes, `# @openspec:` marker syntax, or quoted Gherkin examples — only `name:`, descriptions, and prose.

## 1. Behaviour-driven schema replacement

- [x] 1.1 Replace `openspec/schemas/behaviour-driven/schema.yaml` with the source schema.yaml adapted: `name: behaviour-driven`, description "Behaviour-driven workflow - proposal → specs (Markdown with fenced Gherkin) → design → tasks", prose "behavior"→"behaviour" normalization; artifacts (proposal, specs with `generates: "specs/**/spec.md"`, design, tasks with stack-substitution rules), simple `apply:` block, and byte-identical `format:` block
- [x] 1.2 Replace `openspec/schemas/behaviour-driven/templates/{proposal,spec,design,tasks}.md` with the source templates verbatim (proposal.md spelling normalized); confirm no self-referential `openspec/schemas/behaviour-driven/` path reappears in tasks.md
- [x] 1.3 Replace `openspec/schemas/behaviour-driven/skills.txt` with: acceptance-test-authoring, bdd-zone-check, gherkin-authoring, glossary, openspec-git-discipline
- [x] 1.4 Rewrite `openspec/schemas/behaviour-driven/README.md`: intro (spec-as-source BDD), two rules, spec format with example + `format:` block pointer, flow `proposal -> (specs, design) -> tasks`, supported-stacks table, activation (`schema: behaviour-driven` + `stack:` key + spelling note re upstream `behavior-driven`), validate command, Associated Skills (5 bullets, skills repo link, install-step note)
- [x] 1.5 Run `openspec schema validate behaviour-driven`

## 2. Intent-driven schema upgrade

- [x] 2.1 Edit `openspec/schemas/intent-driven/schema.yaml`: new description; proposal instruction adopts source wording keeping the ADR-distillation paragraph; specs instruction replaced with the fenced-Gherkin instruction (`generates: "specs/**/spec.md"`, new description); design and adr artifacts unchanged; tasks instruction replaced with source stack-substitution instruction plus appended ADR-honouring sentence (`requires: [specs, adr]` unchanged); add `format:` block byte-identical to behaviour-driven's
- [x] 2.2 Replace `openspec/schemas/intent-driven/templates/spec.md` and `templates/proposal.md` with the source versions (spelling normalized); leave `design.md` and `adr.md` untouched
- [x] 2.3 Replace `openspec/schemas/intent-driven/templates/tasks.md` with the source 3-section template plus one added line in section 2 requiring implementation to honour in-force ADRs under `<repo>/adr/`
- [x] 2.4 Replace `openspec/schemas/intent-driven/skills.txt` with 8 alphabetical entries: acceptance-test-authoring, architectural-decision-records, bdd-zone-check, c4-diagrams, gherkin-authoring, glossary, grill-me, openspec-git-discipline
- [x] 2.5 Rewrite `openspec/schemas/intent-driven/README.md`: intro (behaviour-driven + durable ADRs), fit guidance, activation (`schema: intent-driven` + `stack:`), stage gates with flow `proposal -> (specs, design) -> adr -> tasks`, fenced-Gherkin spec format example (drop the no-`.feature`/no-lint paragraph; state extraction + gherkin-lint), acceptance enforcement (two rules + stacks), ADR Persistence kept essentially verbatim, validate command, Associated Skills (8 bullets)
- [x] 2.6 Run `openspec schema validate intent-driven`

## 3. Repository documentation

- [x] 3.1 Update root `README.md`: "Choosing a Schema" table rows for behaviour-driven (fenced-Gherkin executable BDD) and intent-driven (`proposal -> (specs, design) -> adr -> tasks`, behaviour-driven plus durable ADRs); rewrite the relationship paragraph deleting the "intentionally not its executable acceptance-test enforcement" claim; add `stack: javascript  # javascript | python` to the example config; rewrite the Behaviour-Driven and Intent-Driven catalog blurbs and the Spec-Driven With ADR comparison sentence; update the Intent-Driven flow diagram
- [x] 3.2 Add one line to `AGENT_INSTALL.md` Step 4: some schemas require extra `config.yaml` keys — check the schema README (behaviour-driven and intent-driven require `stack: javascript|python`)

## 4. Verification

- [x] 4.1 Run `openspec schema validate` for all five schemas (behaviour-driven, intent-driven, minimalist, event-driven, spec-driven-with-adr)
- [x] 4.2 YAML-parse both edited schema.yaml files and assert their `format:` blocks are identical
- [x] 4.3 Stale-reference greps over the two schema dirs, root README, and AGENT_INSTALL.md: no authored `features/*.feature` workflow references; no `### Requirement:`/`#### Scenario:`/`## ADDED Requirements` in the two schema dirs; old `Cucumber.js`/`gherkin-lint-script` gate language gone while `gherkin-lint` remains in both specs instructions; `behavior-driven` only in the deliberate spelling notes; no `behaviour-driven/` self-reference inside `behaviour-driven/templates/`
- [x] 4.4 Confirm every skills.txt line matches a directory under the canonical skills repo clone (local: `/Users/harikrishnan/polarizer/projects/sdd/openspec/skills/.agents/skills/`)
- [x] 4.5 Confirm flow diagrams in both schema READMEs and the root README match the `requires:` graphs in the two schema.yaml files
- [x] 4.6 Run `openspec validate adopt-fenced-gherkin-behaviour-intent --type change --strict`
