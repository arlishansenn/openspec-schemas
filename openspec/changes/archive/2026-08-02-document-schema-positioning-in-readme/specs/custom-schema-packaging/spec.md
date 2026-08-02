## MODIFIED Requirements

### Requirement: Repository root SHALL provide catalog and install guidance for humans and agents
The repository root SHALL include a `README.md` that explains the purpose of this schema collection, starts the main install section with an agent-oriented prompt that points to the raw root `README.md`, gives a self-install fallback for human readers, includes a dedicated agent-oriented install flow that removes ambiguity about prerequisites and copying the full schema folder, and lists only currently packaged schemas with canonical spec coverage. The install section SHALL also note that schemas declare companion skills in a `skills.txt` manifest and that the install guide's skills step installs those skills into `.agents/skills/` from https://github.com/intent-driven-dev/skills.

The root `README.md` SHALL additionally include a "Choosing a Schema" section, placed before the install section, that positions the upstream `spec-driven` built-in as the default for most projects and `intent-driven` as the most complete general-purpose schema for complex projects, provides a comparison table covering `spec-driven` and every packaged schema with each schema's artifact flow and choose-when guidance, and documents inter-schema relationships accurately: `intent-driven` subsumes `spec-driven-with-adr` (same artifact graph with Gherkin-style specs and a larger skill set); `behaviour-driven` remains the choice for executable BDD enforcement (`.feature` files, Cucumber acceptance gates) which `intent-driven` intentionally excludes; `event-driven` is domain-specific for event-centric/AsyncAPI-first systems; `minimalist` is for small, low-risk changes.

Every schema entry in the root catalog SHALL be nested under a single catalog section at the same heading level and SHALL follow a consistent structure: description, artifact order, activation snippet for `openspec/config.yaml`, validate command, and a link to the schema's README. The `intent-driven` catalog entry SHALL link https://github.com/intent-driven-dev/intent-driven-template as a starter project with the schema pre-installed, while skills references continue to point at https://github.com/intent-driven-dev/skills.

#### Scenario: Human discovers schema options from repo root
- **WHEN** a human user opens the repository root `README.md`
- **THEN** they can find a self-install fallback that explains how to get the repository locally, where schema folders are copied, how a schema is activated in `openspec/config.yaml`, and how to validate the install

#### Scenario: Agent discovers deterministic install flow from repo root
- **WHEN** a coding agent opens the repository root `README.md`
- **THEN** it can find a top-level raw-README prompt handoff plus a dedicated agent-oriented install section that starts with prerequisite checks, explains the early-exit behavior, and tells it to clone the repo locally before copying the full schema directory recursively

#### Scenario: Human discovers the intent-driven schema from the root catalog
- **WHEN** a human user opens the repository root `README.md`
- **THEN** they can find `intent-driven` in the schema catalog
- **AND** they can find a reference to `openspec/schemas/intent-driven/README.md`.

#### Scenario: Agent can install the intent-driven schema from catalog guidance
- **WHEN** a coding agent follows the repository install guidance for `intent-driven`
- **THEN** it can copy the full `openspec/schemas/intent-driven/` folder into a target project's `openspec/schemas/intent-driven/`
- **AND** activate it with `schema: intent-driven`.

#### Scenario: Root catalog excludes removed linearized schema
- **GIVEN** the `linearized` schema package has been removed from `openspec/schemas/linearized/`
- **WHEN** a human or coding agent reads the root `README.md`
- **THEN** the schema catalog no longer lists `linearized` as an available schema
- **AND** the README no longer points readers to `openspec/schemas/linearized/README.md`
- **AND** install examples no longer tell agents to activate `schema: linearized`

#### Scenario: Reader learns about associated skills from the root README
- **WHEN** a human or coding agent reads the "Install a Schema" section of the root `README.md`
- **THEN** they learn that each schema declares companion skills in a `skills.txt` manifest
- **AND** they learn the install guide's skills step installs those skills into `.agents/skills/` from https://github.com/intent-driven-dev/skills

#### Scenario: Reader can choose a schema from the root README
- **WHEN** a human or coding agent reads the "Choosing a Schema" section of the root `README.md`
- **THEN** they learn that `spec-driven` is the upstream default suitable for most projects
- **AND** they learn that `intent-driven` is the most complete general-purpose schema for complex projects
- **AND** they can compare `spec-driven` and all packaged schemas in a table showing each schema's artifact flow and choose-when guidance

#### Scenario: Reader learns accurate inter-schema relationships
- **WHEN** a human or coding agent reads the "Choosing a Schema" section of the root `README.md`
- **THEN** they learn that `intent-driven` subsumes `spec-driven-with-adr`
- **AND** they learn that `intent-driven` adopts behaviour-driven's Gherkin spec style but not its executable acceptance-test enforcement, so `behaviour-driven` remains the choice for `.feature` files and Cucumber gates
- **AND** they learn that `event-driven` targets event-centric/AsyncAPI-first systems and `minimalist` targets small, low-risk changes

#### Scenario: Reader discovers the intent-driven starter template
- **WHEN** a human or coding agent reads the `intent-driven` entry in the root catalog
- **THEN** they can find a link to https://github.com/intent-driven-dev/intent-driven-template described as a starter project with the intent-driven schema already installed
- **AND** skills references continue to point at https://github.com/intent-driven-dev/skills

#### Scenario: Catalog entries are consistent and correctly nested
- **WHEN** a human or coding agent reads the schema catalog in the root `README.md`
- **THEN** every packaged schema, including `spec-driven-with-adr`, appears under the same catalog section at the same heading level
- **AND** every entry provides description, artifact order, activation snippet, validate command, and a link to the schema's README

#### Scenario: Post-apply validation still passes
- **WHEN** the root `README.md` update is applied
- **THEN** no files under `openspec/schemas/` change (no schemas are affected)
- **AND** `openspec schema validate` continues to pass as a sanity check
