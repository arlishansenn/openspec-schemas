# behaviour-driven-schema-workflow Delta

## ADDED Requirements

### Requirement: Behaviour-driven specs SHALL use fenced Gherkin with delta markers
The `behaviour-driven` schema SHALL define specs as Markdown files whose executable content lives exclusively in column-0 ` ```gherkin ` fences, mapped onto OpenSpec concepts (`Feature:` = capability, `Rule:` = requirement, `Scenario:` = example), with delta operations expressed as `# @openspec:` marker comments inside the fences, and SHALL expose a machine-readable `format:` block in `schema.yaml` describing the fence, requirement, scenario, and delta-marker patterns.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Spec files follow the fence contract
- **WHEN** a contributor authors a spec under `specs/<capability>/spec.md`
- **THEN** schema guidance allows prose anywhere but treats only fences opened by ` ```gherkin ` at column 0 (info string exactly `gherkin`) and closed by ` ``` ` at column 0 as executable
- **AND** it requires every `spec.md` to contain at least one gherkin fence
- **AND** it requires all fences in a file to concatenate into exactly one top-level `Feature:`
- **AND** it treats an indented ` ```gherkin ` opener as a hard extraction error.

#### Scenario: Gherkin keywords map onto OpenSpec concepts
- **WHEN** a contributor authors spec content inside a gherkin fence
- **THEN** schema guidance maps `Feature:` to the capability, `Rule:` to one requirement followed by a SHALL/MUST description line, and `Scenario:` to a concrete Given/When/Then example
- **AND** it requires every `Rule:` to have at least one `Scenario:`.

#### Scenario: Delta operations use inline @openspec markers
- **WHEN** a contributor authors a delta spec
- **THEN** schema guidance places `# @openspec: ADDED`, `# @openspec: MODIFIED`, `# @openspec: REMOVED`, or `# @openspec: RENAMED from="<old>" to="<new>"` comments inside a gherkin fence immediately above the `Rule:` they apply to
- **AND** MODIFIED entries copy the entire existing `Rule:` block from `openspec/specs/<capability>/spec.md` before editing so no detail is lost at archive time
- **AND** REMOVED entries need only the `Rule:` line naming the requirement.

#### Scenario: Schema exposes a machine-readable format block
- **WHEN** tooling reads `openspec/schemas/behaviour-driven/schema.yaml`
- **THEN** a `format:` block defines the spec file extension, gherkin fence open/close patterns, requirement (`Rule:`) pattern, scenario pattern, and `@openspec:` delta marker and rename patterns
- **AND** those patterns are kept byte-identical to the upstream behavior-driven template so extraction tooling matches consistently.

#### Scenario: Specs are linted before completion
- **WHEN** a contributor completes a spec artifact
- **THEN** schema guidance requires extracting the Gherkin and linting it with gherkin-lint, fixing every reported issue, per the acceptance-test-authoring skill's stack-specific commands.

### Requirement: Behaviour-driven tasks SHALL scaffold a stack-agnostic acceptance suite
The `behaviour-driven` schema SHALL generate tasks from a three-section template — first-time acceptance-suite setup, one task per pending step definition, and completion — parameterized by a `stack:` key (`javascript` or `python`) in the consuming project's `openspec/config.yaml`.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Tasks substitute the configured stack
- **GIVEN** proposal, specs, and design artifacts are complete
- **WHEN** `tasks.md` is generated
- **THEN** the generator reads `stack:` from `openspec/config.yaml` and substitutes it wherever the template says `<stack>`
- **AND** it inlines the concrete destination filenames from that stack's "Files to copy" table in the acceptance-test-authoring skill's `references/<stack>/SETUP.md`
- **AND** the authored `tasks.md` names real filenames, not placeholders
- **AND** if `stack:` is absent, the first setup task records it before any scaffolding instead of guessing.

#### Scenario: First-time setup scaffolds the acceptance suite once
- **GIVEN** the consuming project has no `acceptance-tests/` directory
- **WHEN** the generated task list is applied
- **THEN** setup tasks create `acceptance-tests/` as an independent project for the configured stack that boots the app before the suite and shuts it down after
- **AND** the runner extracts Gherkin from every `spec.md` under `openspec/` into `acceptance-tests/.extracted/`, excluding `openspec/changes/archive/`
- **AND** the single test command always generates an HTML report under `acceptance-tests/reports/`
- **AND** a spec-lint command and gitignores for `.extracted/` and `reports/` are added
- **AND** the whole setup section is skipped when `acceptance-tests/` already exists.

#### Scenario: Implementation proceeds one pending step at a time
- **WHEN** implementation tasks are generated
- **THEN** each task covers one pending step definition following the cadence: fails for the right reason, then implement until it passes, then commit
- **AND** the completion section requires the full suite to pass with zero pending or undefined steps and an HTML report generated.

### Requirement: Behaviour-driven workflow SHALL follow spec-first and zone-isolation rules
The `behaviour-driven` schema SHALL be governed by two rules: acceptance tests must always pass, and specs and code are never modified together in one unit of work (`tasks.md` exempt).

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: Code without a driving spec delta is reverted
- **GIVEN** code changes exist without a corresponding active spec delta under `openspec/changes/`
- **WHEN** the violation is detected
- **THEN** workflow guidance requires reverting the unspecced code and restarting spec-first rather than patching it until tests pass.

#### Scenario: A unit of work touches one zone
- **WHEN** a contributor works through generated tasks
- **THEN** each unit of work touches either `openspec/` (specs zone) or application code, never both
- **AND** any file named `tasks.md` is exempt from zone checks
- **AND** the bdd-zone-check skill documents the discipline and its reference enforcement.

### Requirement: Behaviour-driven README SHALL document the fenced-Gherkin workflow
The `behaviour-driven` README MUST describe the spec-as-source workflow: the two rules, the fenced-Gherkin spec format, the artifact flow, the supported acceptance stacks, and activation including the `stack:` key and the schema-name spelling note.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: README documents format, flow, and stacks
- **WHEN** a contributor reads the schema README
- **THEN** it documents the two rules (acceptance tests always pass; specs and code never modified together, `tasks.md` exempt)
- **AND** it documents the fenced-Gherkin spec format with a short example and points to the `format:` block in `schema.yaml` as the machine-readable definition
- **AND** it shows the artifact flow `proposal -> (specs, design) -> tasks`
- **AND** it lists the supported stacks: `javascript` (cucumber-js, `reports/cucumber-report.html`) and `python` (behave 1.2.7+, `reports/behave-report.html`) with a shared pinned gherkin-lint configuration.

#### Scenario: README documents activation with the stack key
- **WHEN** a contributor follows the README's activation guidance
- **THEN** it tells them to set `schema: behaviour-driven` and `stack: javascript` or `stack: python` in `openspec/config.yaml`
- **AND** it notes that the acceptance-test-authoring skill's upstream docs show `schema: behavior-driven` (American spelling) while this package uses `behaviour-driven`.

## MODIFIED Requirements

### Requirement: Behaviour-driven schema SHALL enforce proposal-to-tasks workflow with Gherkin-style specs
The `behaviour-driven` schema SHALL expose the artifacts `proposal`, `specs`, `design`, and `tasks`, SHALL generate behaviour specs as Markdown files with fenced Gherkin at `specs/<capability>/spec.md`, and SHALL require those artifacts to be completed in dependency order before apply readiness.

#### Scenario: Workflow proceeds in dependency order
- **GIVEN** a project activates `schema: behaviour-driven`
- **WHEN** a new OpenSpec change is created
- **THEN** `specs` and `design` each require only `proposal` and may proceed in parallel
- **AND** `tasks` requires both `specs` and `design`, giving the flow `proposal -> (specs, design) -> tasks`.

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
- **WHEN** `proposal`, `specs`, or `design` is incomplete
- **THEN** `tasks` remains blocked until all required predecessor artifacts are complete.

## REMOVED Requirements

### Requirement: Behaviour-driven schema SHALL drive a feature-file acceptance workflow
**Reason**: The workflow no longer maintains committed root-level `features/*.feature` files; Gherkin lives in `spec.md` fences and is extracted into gitignored `acceptance-tests/.extracted/` at test time.

**Migration**: Delete committed `features/` directories in consuming projects; author specs as fenced Gherkin per the new specs requirement and let the acceptance runner extract them.

### Requirement: Behaviour-driven tasks SHALL be generated from a gate contract
**Reason**: The four-gate template (feature extraction, failing acceptance, scenario slices, verification) is replaced by the three-section stack-agnostic template (first-time setup, one task per pending step, completion).

**Migration**: Generate tasks from the new template; the red→green→commit cadence per pending step replaces scenario-slice gates.

### Requirement: Behaviour-driven commit gates SHALL be mandatory
**Reason**: Gate-boundary commit checkpoints are superseded by the per-step red→green→commit cadence and the zone-isolation rule, which ties commits to units of work.

**Migration**: Commit after each passing step definition; keep specs-zone and code-zone commits separate per the spec-first and zone-isolation rules.

### Requirement: Behaviour-driven acceptance-test project SHALL follow Node and JavaScript hygiene
**Reason**: The schema is now stack-agnostic (`javascript` or `python`); stack-specific project hygiene is owned by the acceptance-test-authoring skill's per-stack reference packs.

**Migration**: Follow the "Files to copy" table and setup guidance in the acceptance-test-authoring skill for the configured stack.

### Requirement: Behaviour-driven acceptance tests SHALL generate HTML reports
**Reason**: Folded into the stack-agnostic acceptance-suite requirement — the single test command always writes an HTML report under `acceptance-tests/reports/` for both stacks.

**Migration**: None needed; HTML reporting remains mandatory via the first-time setup tasks and completion gate.

### Requirement: Behaviour-driven acceptance execution SHALL be bounded
**Reason**: User-selected attempt limits and per-attempt checkboxes are dropped; the workflow proceeds one pending step at a time and stops naturally when a step cannot be made to pass.

**Migration**: Remove attempt-limit bookkeeping from task generation; use the red→green→commit cadence per step.

### Requirement: Behaviour-driven acceptance tests SHALL not be weakened after the failing-test checkpoint
**Reason**: Superseded by the spec-first and zone-isolation rules: tests derive from specs, code without a driving spec delta is reverted, and spec edits are a separate unit of work from code edits.

**Migration**: Enforce integrity via the two rules (bdd-zone-check skill); behaviour corrections go through a spec delta, never through test edits during implementation.

### Requirement: Behaviour-driven README SHALL position the schema as a demonstration baseline
**Reason**: Replaced by the fenced-Gherkin README requirement; the README now documents a portable two-rule workflow with supported stacks rather than a demonstration-quality Node-only baseline with customization boundaries.

**Migration**: Rewrite the README per the new README requirement.

### Requirement: Behaviour-driven schema SHALL guide Gherkin-style authoring inside Markdown wrappers
**Reason**: The OpenSpec Markdown wrapper (`## ADDED Requirements`, `### Requirement:`, `#### Scenario:`) is no longer the spec format; fenced Gherkin with `# @openspec:` markers replaces it.

**Migration**: Author specs per the fenced-Gherkin requirement; convert existing wrapper-format specs by moving each requirement into a `Rule:` with its scenarios inside a gherkin fence.
