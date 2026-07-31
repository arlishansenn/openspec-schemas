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
