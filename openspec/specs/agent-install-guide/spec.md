# agent-install-guide Specification

## Purpose

Define the behavior of the standalone `AGENT_INSTALL.md` guide that coding agents follow to install a schema from this repository into a target OpenSpec project.

## Requirements

### Requirement: Install flow SHALL install a schema's associated skills
`AGENT_INSTALL.md` SHALL include a step, after validation, that installs the skills declared in the installed schema's `skills.txt` manifest into the target project's `.agents/skills/`, sourcing them from https://github.com/intent-driven-dev/skills.

#### Scenario: Schema without a manifest skips skill installation
- **GIVEN** an agent has completed the install steps through validation
- **WHEN** no `skills.txt` exists at `./openspec/schemas/<schema-name>/` (Option A) or `$HOME/.openspec/schemas/<schema-name>/` (Option B)
- **THEN** the guide instructs the agent to skip the skills step because the schema has no associated skills

#### Scenario: Manifest skills are copied from a fresh shallow clone
- **GIVEN** the installed schema has a `skills.txt` manifest
- **WHEN** the agent performs the skills step
- **THEN** the guide instructs the agent to clone https://github.com/intent-driven-dev/skills with `--depth 1` into a temporary directory, removing any stale clone at that path first
- **AND** copy `.agents/skills/<skill-name>` from the clone into `./.agents/skills/<skill-name>` for each skill named in the manifest
- **AND** finish by reporting the list of installed skills and their destination

#### Scenario: Existing skill in target project is not overwritten silently
- **GIVEN** a skill named in the manifest already exists in the target project's `./.agents/skills/`
- **WHEN** the agent processes that skill
- **THEN** the guide instructs the agent to ask the user whether to replace or keep their copy instead of overwriting silently

#### Scenario: Skill missing from the clone is reported and skipped
- **GIVEN** a skill named in the manifest does not exist in the skills repo clone
- **WHEN** the agent processes that skill
- **THEN** the guide instructs the agent to report the missing skill to the user and continue with the remaining skills

### Requirement: Guide SHALL explain the skills manifest convention
`AGENT_INSTALL.md` SHALL explain, in its intro or the skills step preamble, that schemas declare their companion skills in a `skills.txt` manifest inside the schema directory and that skills are sourced from https://github.com/intent-driven-dev/skills.

#### Scenario: Reader learns where installed skills come from
- **WHEN** a reader opens `AGENT_INSTALL.md`
- **THEN** they can find a sentence explaining the `skills.txt` manifest convention and the canonical skills repository

### Requirement: Install guide SHALL be a standalone agent-agnostic file
The repository SHALL keep the AI agent install instructions in a single standalone `AGENT_INSTALL.md` file at the repository root, shared by all agents, so that the README stays focused and the install flow has one authoritative source.

#### Scenario: Standalone guide exists at the repository root
- **GIVEN** the repository root
- **WHEN** a reader looks for agent install instructions
- **THEN** an `AGENT_INSTALL.md` file at the repository root contains the full step-by-step install flow for any schema in this repository
- **AND** the file is agent-agnostic (no instruction is tied to a specific coding agent)
- **AND** the file name does not collide with the `AGENTS.md` convention used for general agent/project instructions

### Requirement: Guide SHALL enumerate schemas and require picking exactly one
`AGENT_INSTALL.md` SHALL instruct the agent to enumerate every available schema and have the user choose exactly one to install and enable.

#### Scenario: Agent enumerates schemas and the user picks one
- **GIVEN** an agent is following `AGENT_INSTALL.md`
- **WHEN** the agent reaches the schema-selection step
- **THEN** the flow instructs the agent to enumerate all available schemas by listing the directories under `openspec/schemas/` in this repository
- **AND** the agent presents that list and asks the user to pick exactly one schema to install and enable
- **AND** the agent proceeds with the clone/copy and activation steps only after the user has chosen exactly one schema
- **AND** if the user named a schema up front, the agent confirms it matches one of the enumerated schemas before proceeding

### Requirement: README SHALL delegate install steps to the guide
The README "Install a Schema" section SHALL point agents at `AGENT_INSTALL.md` with a short trigger prompt rather than embedding detailed install steps, so that no install step is documented in two places.

#### Scenario: README trigger points the agent at the guide
- **GIVEN** the README "Install a Schema" section
- **WHEN** a reader reads the agent trigger prompt
- **THEN** it instructs the agent to read `AGENT_INSTALL.md` and follow the instructions, without embedding the detailed install steps inline
- **AND** the trigger does not require the user to name a schema in the prompt (the guide enumerates schemas and asks)

#### Scenario: README contains no duplicated install flow
- **GIVEN** the README
- **WHEN** a reader reaches the agent install content
- **THEN** the embedded step-by-step flow is removed and replaced by a pointer to `AGENT_INSTALL.md`
- **AND** no install step is documented in two places
