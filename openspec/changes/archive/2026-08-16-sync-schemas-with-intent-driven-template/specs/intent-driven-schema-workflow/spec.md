## MODIFIED Requirements

### Requirement: Repository SHALL package the intent-driven schema
The repository SHALL provide a reusable `intent-driven` OpenSpec schema package for teams that want proposal-led intent capture, behaviour specs as OpenSpec Markdown deltas, technical design, durable architecture decision records, and implementation tasks. The executable-acceptance workflow is not part of the schema; it is provided by the `spec-as-source` companion skill that the schema declares.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Self-contained intent-driven schema folder is available
- **WHEN** the `intent-driven` schema is added
- **THEN** it is available as a self-contained folder at `openspec/schemas/intent-driven/`
- **AND** the folder contains `schema.yaml`, a schema `README.md`, `skills.txt`, and all templates referenced by the schema.

#### Scenario: Intent-driven schema can be activated
- **WHEN** a contributor reads the `intent-driven` schema README
- **THEN** it tells them to set `schema: intent-driven` in `openspec/config.yaml`
- **AND** it explains when the schema is suitable and unsuitable
- **AND** it does not require a `stack:` key for activation.

### Requirement: Intent-driven schema SHALL enforce proposal-to-tasks workflow with ADRs
The `intent-driven` schema SHALL expose the artifacts `proposal`, `specs`, `design`, `adr`, and `tasks`, SHALL generate behaviour specs as OpenSpec Markdown delta files under `specs/` (the artifact `generates` pattern is `specs/**/*.md`), and SHALL require those artifacts to be completed in dependency order before apply readiness.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Workflow proceeds in dependency order
- **GIVEN** a project activates `schema: intent-driven`
- **WHEN** a new OpenSpec change is created
- **THEN** `specs` and `design` each require only `proposal` and may proceed in parallel
- **AND** `adr` requires `design`, and `tasks` requires `specs` and `adr`, giving the flow `proposal -> (specs, design) -> adr -> tasks`.

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
- **WHEN** `proposal`, `specs`, `design`, or `adr` is incomplete
- **THEN** `tasks` remains blocked until all required predecessor artifacts are complete.

#### Scenario: Documented flow matches the machine-readable dependency graph
- **WHEN** a contributor compares `openspec/schemas/intent-driven/README.md` with `openspec/schemas/intent-driven/schema.yaml`
- **THEN** the artifact flow shown in the README matches the `requires:` declarations in `schema.yaml`
- **AND** the README does not describe `specs` and `design` as sequential.

## REMOVED Requirements

### Requirement: Intent-driven specs SHALL use fenced Gherkin with delta markers
**Reason**: The spec format moves out of the schema. `intent-driven-template` now packages `intent-driven` as vanilla OpenSpec and delegates fenced-Gherkin authoring to the opt-in `spec-as-source` skill, which nests Gherkin fences inside standard OpenSpec headings and therefore needs no schema-level `format:` block.

**Migration**: Projects that author fenced-Gherkin specs install the `spec-as-source` skill from https://github.com/intent-driven-dev/skills and draft `spec.md` from its `references/spec.md` instead of the schema template. Projects that do not adopt the skill use the schema's OpenSpec Markdown delta format.

### Requirement: Intent-driven tasks SHALL scaffold a stack-agnostic acceptance suite honouring ADRs
**Reason**: Acceptance-suite scaffolding is owned by the `spec-as-source` and `acceptance-test-authoring` skills, not the schema. The schema's task template returns to the template project's generic task-group form, so the schema no longer depends on a `stack:` key.

**Migration**: Projects that want acceptance-first task ordering install the `spec-as-source` skill and draft `tasks.md` from its `references/tasks.md`, which retains the first-time-setup, one-task-per-pending-step, and completion sections keyed on `stack:`.

### Requirement: Intent-driven README SHALL document the acceptance-enforced workflow
**Reason**: The README no longer documents acceptance enforcement or the `stack:` key as schema properties, because both moved to the `spec-as-source` skill.

**Migration**: Replaced by "Intent-driven README SHALL document the workflow and its companion skills" below.

## ADDED Requirements

### Requirement: Intent-driven specs SHALL use OpenSpec Markdown delta headers
The `intent-driven` schema SHALL define specs as OpenSpec Markdown delta files that archive can merge, using `## ADDED Requirements`, `## MODIFIED Requirements`, `## REMOVED Requirements`, and `## RENAMED Requirements` section headers, `### Requirement: <name>` for each requirement with a SHALL/MUST description, and `#### Scenario: <name>` with GIVEN/WHEN/THEN steps for each scenario. The schema SHALL NOT declare a `format:` block, and SHALL NOT embed fenced-Gherkin extraction or linting rules.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Spec files use OpenSpec delta headers
- **WHEN** a contributor authors a spec under `specs/<capability>/spec.md`
- **THEN** schema guidance requires `## ADDED Requirements`, `## MODIFIED Requirements`, `## REMOVED Requirements`, or `## RENAMED Requirements` section headers
- **AND** each requirement uses `### Requirement: <name>` followed by a SHALL/MUST description
- **AND** each scenario uses exactly four hashtags (`#### Scenario: <name>`) with GIVEN/WHEN/THEN steps
- **AND** every requirement has at least one scenario
- **AND** `MODIFIED` entries copy the entire existing requirement block before editing.

#### Scenario: Schema declares no format block
- **WHEN** tooling reads `openspec/schemas/intent-driven/schema.yaml`
- **THEN** the file contains no `format:` block
- **AND** default OpenSpec requirement and scenario parsing applies.

#### Scenario: Schema templates match the template project
- **WHEN** a contributor compares `openspec/schemas/intent-driven/templates/` with the same directory in https://github.com/intent-driven-dev/intent-driven-template
- **THEN** `proposal.md`, `spec.md`, `design.md`, `adr.md`, and `tasks.md` are identical.

### Requirement: Intent-driven tasks SHALL use a generic task template honouring ADRs
The `intent-driven` schema SHALL generate `tasks.md` from a generic numbered task-group template with `- [ ] X.Y` checkboxes, SHALL NOT reference a `stack:` key or acceptance-suite scaffolding, and SHALL direct implementation to reference specs for what to build, design for how to build it, and currently in-force ADRs under `<repo>/adr/` for durable architectural commitments to honour.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Task template is generic
- **GIVEN** `specs` and `adr` artifacts are complete
- **WHEN** `tasks.md` is generated
- **THEN** the template provides numbered task groups with `- [ ] X.Y` checkboxes and no acceptance-suite setup section
- **AND** task guidance does not read `stack:` from `openspec/config.yaml`.

#### Scenario: Implementation honours in-force ADRs
- **WHEN** `tasks.md` is generated
- **THEN** task guidance references specs for what to build, design for how to build it, and `<repo>/adr/` for durable architectural commitments to honour.

### Requirement: Intent-driven README SHALL document the workflow and its companion skills
The `intent-driven` README MUST describe the schema as a proposal-to-tasks workflow capturing intent, behaviour, design, and durable decisions; document the OpenSpec Markdown delta spec format; show the artifact flow `proposal -> (specs, design) -> adr -> tasks`; document activation as `schema: intent-driven` without a `stack:` key; and retain an "Associated Skills" section listing exactly the skills in the schema's `skills.txt`. Acceptance testing, fenced Gherkin, and specs/code zone isolation MUST be attributed to the `spec-as-source` skill rather than to the schema.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: README documents format, flow, and fit
- **WHEN** a contributor reads `openspec/schemas/intent-driven/README.md`
- **THEN** it describes the schema as a proposal-to-tasks workflow for changes needing intent, behaviour, design, and durable architectural decisions
- **AND** it documents the OpenSpec Markdown delta spec format with an example
- **AND** it shows the artifact flow `proposal -> (specs, design) -> adr -> tasks`
- **AND** it documents activation as `schema: intent-driven` with no `stack:` key
- **AND** it explains when `behaviour-driven` is the better fit.

#### Scenario: README attributes the acceptance workflow to the skill
- **WHEN** a contributor reads `openspec/schemas/intent-driven/README.md`
- **THEN** fenced-Gherkin authoring, acceptance-test execution, and specs/code zone isolation are described as provided by the `spec-as-source` skill
- **AND** the README links to https://github.com/intent-driven-dev/skills
- **AND** the README does not describe acceptance enforcement as a property of the schema.

#### Scenario: README lists associated skills
- **WHEN** a contributor reads the "Associated Skills" section of `openspec/schemas/intent-driven/README.md`
- **THEN** it lists exactly the skills in `openspec/schemas/intent-driven/skills.txt`, each with a one-line purpose
- **AND** it notes that the install guide's skills step installs them into `.agents/skills/`.

### Requirement: Intent-driven realignment SHALL pass schema validation
Any change that realigns `openspec/schemas/intent-driven/` with the template project SHALL be verified by OpenSpec validation before completion.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Schema validation passes after realignment
- **WHEN** implementation changes files under `openspec/schemas/intent-driven/`
- **THEN** `openspec schema validate intent-driven` passes before the change is considered complete.

#### Scenario: Strict change validation passes
- **WHEN** the realignment updates OpenSpec change artifacts
- **THEN** `openspec validate <change-name> --type change --strict` passes before the change is considered complete.
