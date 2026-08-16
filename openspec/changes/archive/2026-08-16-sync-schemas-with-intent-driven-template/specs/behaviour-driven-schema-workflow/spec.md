## MODIFIED Requirements

### Requirement: Repository SHALL package the behaviour-driven schema
The repository SHALL provide a reusable `behaviour-driven` OpenSpec schema package for teams that want proposal-led changes with observable behaviour captured as Gherkin-style scenarios inside OpenSpec-mergeable Markdown specs. The executable-acceptance workflow is not part of the schema; it is provided by the `spec-as-source` companion skill that the schema declares.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Self-contained behaviour-driven schema folder is available
- **WHEN** the `behaviour-driven` schema is added
- **THEN** it is available as a self-contained folder at `openspec/schemas/behaviour-driven/`
- **AND** the folder contains `schema.yaml`, a schema `README.md`, `skills.txt`, and all templates referenced by the schema.

#### Scenario: Behaviour-driven schema can be activated
- **WHEN** a contributor reads the `behaviour-driven` schema README
- **THEN** it tells them to set `schema: behaviour-driven` in `openspec/config.yaml`
- **AND** it explains when the schema is suitable and unsuitable
- **AND** it does not require a `stack:` key for activation.

### Requirement: Behaviour-driven schema SHALL enforce proposal-to-tasks workflow with Gherkin-style specs
The `behaviour-driven` schema SHALL expose the artifacts `proposal`, `specs`, `design`, and `tasks`, SHALL generate behaviour specs as OpenSpec Markdown delta files under `specs/` (the artifact `generates` pattern is `specs/**/*.md`), and SHALL require those artifacts to be completed in dependency order before apply readiness.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Workflow proceeds in dependency order
- **GIVEN** a project activates `schema: behaviour-driven`
- **WHEN** a new OpenSpec change is created
- **THEN** `specs` and `design` each require only `proposal` and may proceed in parallel
- **AND** `tasks` requires both `specs` and `design`, giving the flow `proposal -> (specs, design) -> tasks`.

#### Scenario: Specs generate OpenSpec mergeable Markdown by capability
- **GIVEN** a proposal lists new or modified capabilities
- **WHEN** the `specs` artifact is created
- **THEN** each listed capability is specified at `specs/<capability>/spec.md`
- **AND** the artifact `generates` pattern is `specs/**/*.md`
- **AND** each spec file expresses its delta with `## ADDED Requirements`, `## MODIFIED Requirements`, `## REMOVED Requirements`, or `## RENAMED Requirements` headers so the change can be merged into `openspec/specs/<capability>/spec.md`.

#### Scenario: Tasks are required for apply readiness
- **GIVEN** the schema defines apply readiness
- **WHEN** `tasks` is incomplete
- **THEN** the change is not ready to apply.

#### Scenario: Tasks are blocked by predecessor artifacts
- **GIVEN** the schema defines task dependencies
- **WHEN** `proposal`, `specs`, or `design` is incomplete
- **THEN** `tasks` remains blocked until all required predecessor artifacts are complete.

### Requirement: Behaviour-driven schema SHALL validate cleanly
Changes adding or modifying `openspec/schemas/behaviour-driven/` SHALL pass OpenSpec schema validation before completion.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Schema validation passes
- **WHEN** implementation changes files under `openspec/schemas/behaviour-driven/`
- **THEN** `openspec schema validate behaviour-driven` passes before the change is considered complete.

#### Scenario: Strict change validation is included when behaviour specs change
- **WHEN** a behaviour-driven schema change also updates OpenSpec change artifacts
- **THEN** the verification plan includes `openspec validate <change-name> --type change --strict`
- **AND** the change is not complete until that validation passes.

## REMOVED Requirements

### Requirement: Behaviour-driven specs SHALL use fenced Gherkin with delta markers
**Reason**: The fenced-Gherkin spec format and its `format:` block move out of the schema, matching the separation `intent-driven-template` applied to `intent-driven`. Fenced-Gherkin authoring is now owned by the opt-in `spec-as-source` skill, which nests Gherkin fences inside standard OpenSpec headings and therefore needs no schema-level `format:` block.

**Migration**: Projects that author fenced-Gherkin specs install the `spec-as-source` skill from https://github.com/intent-driven-dev/skills and draft `spec.md` from its `references/spec.md` instead of the schema template. Projects that do not adopt the skill use the schema's OpenSpec Markdown delta format.

### Requirement: Behaviour-driven tasks SHALL scaffold a stack-agnostic acceptance suite
**Reason**: Acceptance-suite scaffolding is owned by the `spec-as-source` and `acceptance-test-authoring` skills, not the schema. The schema's task template becomes generic and no longer depends on a `stack:` key in `openspec/config.yaml`.

**Migration**: Projects that want acceptance-first task ordering install the `spec-as-source` skill and draft `tasks.md` from its `references/tasks.md`, which retains the first-time-setup, one-task-per-pending-step, and completion sections keyed on `stack:`.

### Requirement: Behaviour-driven workflow SHALL follow spec-first and zone-isolation rules
**Reason**: The two rules and the specs/code zone isolation they govern are defined by the `spec-as-source` skill, which absorbed the retired `bdd-zone-check` skill. Stating them as schema requirements duplicates the skill and drifts from it.

**Migration**: Projects that want spec-first discipline and zone isolation install the `spec-as-source` skill, whose "BDD Zone Rules" section is the single source of truth for both rules.

### Requirement: Behaviour-driven README SHALL document the fenced-Gherkin workflow
**Reason**: The README no longer documents the fenced-Gherkin format, the two rules, the `format:` block, or the `stack:` key as schema properties, because all four moved to the `spec-as-source` skill.

**Migration**: Replaced by "Behaviour-driven README SHALL document the workflow and its companion skills" below.

## ADDED Requirements

### Requirement: Behaviour-driven specs SHALL use OpenSpec Markdown delta headers
The `behaviour-driven` schema SHALL define specs as OpenSpec Markdown delta files that archive can merge, using `## ADDED Requirements`, `## MODIFIED Requirements`, `## REMOVED Requirements`, and `## RENAMED Requirements` section headers, `### Requirement: <name>` for each requirement with a SHALL/MUST description, and `#### Scenario: <name>` with GIVEN/WHEN/THEN steps for each scenario. The schema SHALL NOT declare a `format:` block, and SHALL NOT embed fenced-Gherkin extraction or linting rules.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Spec files use OpenSpec delta headers
- **WHEN** a contributor authors a spec under `specs/<capability>/spec.md`
- **THEN** schema guidance requires `## ADDED Requirements`, `## MODIFIED Requirements`, `## REMOVED Requirements`, or `## RENAMED Requirements` section headers
- **AND** each requirement uses `### Requirement: <name>` followed by a SHALL/MUST description
- **AND** each scenario uses exactly four hashtags (`#### Scenario: <name>`) with GIVEN/WHEN/THEN steps
- **AND** every requirement has at least one scenario
- **AND** `MODIFIED` entries copy the entire existing requirement block before editing.

#### Scenario: Schema declares no format block
- **WHEN** tooling reads `openspec/schemas/behaviour-driven/schema.yaml`
- **THEN** the file contains no `format:` block
- **AND** default OpenSpec requirement and scenario parsing applies.

#### Scenario: Spec format matches the intent-driven schema
- **WHEN** a contributor compares the `specs` artifact instructions and `templates/spec.md` of `behaviour-driven` and `intent-driven`
- **THEN** both describe the same OpenSpec Markdown delta format
- **AND** neither declares a `format:` block.

### Requirement: Behaviour-driven tasks SHALL use a generic task template
The `behaviour-driven` schema SHALL generate `tasks.md` from a generic numbered task-group template with `- [ ] X.Y` checkboxes, and SHALL NOT reference a `stack:` key, acceptance-suite scaffolding, or red-green-commit cadence.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Task template is generic
- **GIVEN** `specs` and `design` artifacts are complete
- **WHEN** `tasks.md` is generated
- **THEN** the template provides numbered task groups with `- [ ] X.Y` checkboxes and no acceptance-suite setup section
- **AND** task guidance does not read `stack:` from `openspec/config.yaml`.

### Requirement: Behaviour-driven README SHALL document the workflow and its companion skills
The `behaviour-driven` README MUST describe the schema as a proposal-to-tasks workflow capturing observable behaviour; document the OpenSpec Markdown delta spec format; show the artifact flow `proposal -> (specs, design) -> tasks`; document activation as `schema: behaviour-driven` without a `stack:` key; and retain an "Associated Skills" section listing exactly the skills in the schema's `skills.txt`. Fenced Gherkin, acceptance-test execution, and specs/code zone isolation MUST be attributed to the `spec-as-source` skill rather than to the schema.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: README documents format, flow, and fit
- **WHEN** a contributor reads `openspec/schemas/behaviour-driven/README.md`
- **THEN** it describes the schema as a proposal-to-tasks workflow for changes with meaningful observable behaviour
- **AND** it documents the OpenSpec Markdown delta spec format with a short example
- **AND** it shows the artifact flow `proposal -> (specs, design) -> tasks`
- **AND** it documents activation as `schema: behaviour-driven` with no `stack:` key
- **AND** it explains that `intent-driven` is the same workflow plus durable ADRs.

#### Scenario: README attributes the acceptance workflow to the skill
- **WHEN** a contributor reads `openspec/schemas/behaviour-driven/README.md`
- **THEN** fenced-Gherkin authoring, acceptance-test execution, and the two spec-first rules are described as provided by the `spec-as-source` skill
- **AND** the README links to https://github.com/intent-driven-dev/skills
- **AND** the README no longer references the retired `bdd-zone-check` skill.

#### Scenario: README lists associated skills
- **WHEN** a contributor reads the "Associated Skills" section of `openspec/schemas/behaviour-driven/README.md`
- **THEN** it lists exactly the skills in `openspec/schemas/behaviour-driven/skills.txt`, each with a one-line purpose
- **AND** it notes that the install guide's skills step installs them into `.agents/skills/`.
