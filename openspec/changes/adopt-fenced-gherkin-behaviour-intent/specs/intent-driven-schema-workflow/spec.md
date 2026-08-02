# intent-driven-schema-workflow Delta

## ADDED Requirements

### Requirement: Intent-driven specs SHALL use fenced Gherkin with delta markers
The `intent-driven` schema SHALL adopt the same fenced-Gherkin spec format as `behaviour-driven`: executable content exclusively in column-0 ` ```gherkin ` fences (`Feature:` = capability, `Rule:` = requirement with a SHALL/MUST description, `Scenario:` = Given/When/Then example, every `Rule:` with at least one `Scenario:`), delta operations as `# @openspec:` marker comments inside the fences, gherkin-lint before spec completion, and a `format:` block in `schema.yaml` byte-identical to the `behaviour-driven` schema's.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Spec files follow the shared fence contract
- **WHEN** a contributor authors a spec under `specs/<capability>/spec.md`
- **THEN** schema guidance applies the same fence contract as `behaviour-driven`: prose anywhere, only column-0 ` ```gherkin ` fences executable, at least one fence per file, all fences concatenating into exactly one `Feature:`, indented openers a hard error
- **AND** delta operations use `# @openspec: ADDED|MODIFIED|REMOVED|RENAMED` markers placed immediately above the `Rule:` they apply to
- **AND** MODIFIED entries copy the entire existing `Rule:` block before editing
- **AND** the spec is extracted and linted with gherkin-lint before the artifact is considered complete.

#### Scenario: Format blocks stay in sync across the two schemas
- **WHEN** tooling reads `openspec/schemas/intent-driven/schema.yaml`
- **THEN** its `format:` block is byte-identical to the `behaviour-driven` schema's `format:` block.

### Requirement: Intent-driven tasks SHALL scaffold a stack-agnostic acceptance suite honouring ADRs
The `intent-driven` schema SHALL generate tasks from the same three-section stack-driven template as `behaviour-driven` (first-time acceptance-suite setup keyed on `stack:` in `openspec/config.yaml`, one red→green→commit task per pending step definition, completion with a fully green suite), and SHALL additionally direct implementation to honour currently in-force ADRs under `<repo>/adr/`.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: Tasks follow the stack-driven acceptance template
- **GIVEN** `specs` and `adr` artifacts are complete
- **WHEN** `tasks.md` is generated
- **THEN** the generator reads `stack:` from `openspec/config.yaml` (`javascript` or `python`), substitutes `<stack>` in the template, and inlines the concrete destination filenames from the acceptance-test-authoring skill's `references/<stack>/SETUP.md`
- **AND** if `stack:` is absent, the first setup task records it before any scaffolding
- **AND** the first-time setup section is skipped when `acceptance-tests/` already exists
- **AND** implementation tasks proceed one pending step definition at a time with a red→green→commit cadence
- **AND** completion requires the full suite green with zero pending or undefined steps and an HTML report.

#### Scenario: Implementation honours in-force ADRs
- **WHEN** `tasks.md` is generated
- **THEN** task guidance references specs for what to build, design for how to build it, and `<repo>/adr/` for durable architectural commitments to honour.

### Requirement: Intent-driven README SHALL document the acceptance-enforced workflow
The `intent-driven` README MUST position the schema as `behaviour-driven` plus durable ADRs and document the fenced-Gherkin format, acceptance enforcement, supported stacks, and activation including the `stack:` key.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: README documents format, enforcement, and fit
- **WHEN** a contributor reads `openspec/schemas/intent-driven/README.md`
- **THEN** it describes the schema as `behaviour-driven` plus per-change ADR review and durable ADR records
- **AND** it documents the fenced-Gherkin spec format with an example and states that Gherkin is extracted to `.feature` files at test time and linted with gherkin-lint
- **AND** it documents the two rules and the supported stacks (`javascript` via cucumber-js, `python` via behave)
- **AND** it shows the artifact flow `proposal -> (specs, design) -> adr -> tasks`
- **AND** activation guidance covers `schema: intent-driven` and `stack: javascript` or `stack: python`
- **AND** it no longer claims that `.feature` files must not be created or that Gherkin linting is outside the schema.

## MODIFIED Requirements

### Requirement: Repository SHALL package the intent-driven schema
The repository SHALL provide a reusable `intent-driven` OpenSpec schema package for teams that want proposal-led intent capture, executable fenced-Gherkin behaviour specs, technical design, durable architecture decision records, and implementation tasks driven by an acceptance-test suite.

#### Scenario: Self-contained intent-driven schema folder is available
- **WHEN** the `intent-driven` schema is added
- **THEN** it is available as a self-contained folder at `openspec/schemas/intent-driven/`
- **AND** the folder contains `schema.yaml`, a schema `README.md`, and all templates referenced by the schema.

#### Scenario: Intent-driven schema can be activated
- **WHEN** a contributor reads the `intent-driven` schema README
- **THEN** it tells them to set `schema: intent-driven` in `openspec/config.yaml`
- **AND** it explains when the schema is suitable and unsuitable.

### Requirement: Intent-driven schema SHALL enforce proposal-to-tasks workflow with ADRs
The `intent-driven` schema SHALL expose the artifacts `proposal`, `specs`, `design`, `adr`, and `tasks`, SHALL generate behaviour specs as Markdown files with fenced Gherkin at `specs/<capability>/spec.md`, and SHALL require those artifacts to be completed in dependency order before apply readiness.

#### Scenario: Workflow proceeds in dependency order
- **GIVEN** a project activates `schema: intent-driven`
- **WHEN** a new OpenSpec change is created
- **THEN** `specs` and `design` each require only `proposal` and may proceed in parallel
- **AND** `adr` requires `design`, and `tasks` requires `specs` and `adr`, giving the flow `proposal -> (specs, design) -> adr -> tasks`.

#### Scenario: Specs generate fenced-Gherkin Markdown by capability
- **GIVEN** a proposal lists new or modified capabilities
- **WHEN** the `specs` artifact is created
- **THEN** each listed capability is specified at `specs/<capability>/spec.md` (the artifact `generates` pattern is `specs/**/spec.md`)
- **AND** each spec file expresses its delta with `# @openspec:` markers inside gherkin fences so the change can be merged into `openspec/specs/<capability>/spec.md`.

#### Scenario: Tasks are required for apply readiness
- **GIVEN** the schema defines apply readiness
- **WHEN** `tasks` is incomplete
- **THEN** the change is not ready to apply.

#### Scenario: Tasks are blocked by predecessor artifacts
- **GIVEN** the schema defines task dependencies
- **WHEN** `proposal`, `specs`, `design`, or `adr` is incomplete
- **THEN** `tasks` remains blocked until all required predecessor artifacts are complete.

## REMOVED Requirements

### Requirement: Intent-driven schema SHALL guide Gherkin-style authoring inside Markdown wrappers
**Reason**: The OpenSpec Markdown wrapper (`## ADDED Requirements`, `### Requirement:`, `#### Scenario:`) is no longer the spec format; the schema adopts `behaviour-driven`'s fenced-Gherkin format with `# @openspec:` markers.

**Migration**: Author specs per the fenced-Gherkin requirement; convert existing wrapper-format specs by moving each requirement into a `Rule:` with its scenarios inside a gherkin fence.
