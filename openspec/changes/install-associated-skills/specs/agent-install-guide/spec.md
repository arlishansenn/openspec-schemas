## ADDED User Stories

### User Story: Install associated skills during schema install
As a user installing a schema via my coding agent, I want the install flow to also install the skills the schema depends on, so that the schema arrives with its companion skills instead of requiring a separate manual setup.

#### Acceptance Criteria
- **Given** an agent has completed `AGENT_INSTALL.md` Steps 1–5 for a schema
- **When** the agent reaches "Step 6 — Install Associated Skills"
- **Then** the guide instructs the agent to check for a manifest at `./openspec/schemas/<schema-name>/skills.txt` (Option A install) or `$HOME/.openspec/schemas/<schema-name>/skills.txt` (Option B install)
- **And** if no manifest exists, the guide instructs the agent to skip the step because the schema has no associated skills

#### Acceptance Criteria
- **Given** the installed schema has a `skills.txt` manifest
- **When** the agent performs Step 6
- **Then** the guide instructs the agent to clone https://github.com/intent-driven-dev/skills with `--depth 1` into a temporary directory (removing or replacing any stale clone at that path)
- **And** for each skill named in the manifest, copy `.agents/skills/<skill-name>` from the clone into the target project's `./.agents/skills/<skill-name>`
- **And** finish by reporting the list of installed skills and their destination

#### Acceptance Criteria
- **Given** a skill named in the manifest already exists in the target project's `./.agents/skills/`
- **When** the agent processes that skill in Step 6
- **Then** the guide instructs the agent not to overwrite it silently and to ask the user whether to replace or keep their copy

#### Acceptance Criteria
- **Given** a skill named in the manifest does not exist in the skills repo clone
- **When** the agent processes that skill in Step 6
- **Then** the guide instructs the agent to report the missing skill to the user and continue with the remaining skills

### User Story: Install guide explains the skills manifest convention
As a reader of `AGENT_INSTALL.md`, I want a short explanation that schemas declare their companion skills in `skills.txt` sourced from the canonical skills repo, so that I understand where installed skills come from.

#### Acceptance Criteria
- **Given** `AGENT_INSTALL.md`
- **When** I read the intro or the Step 6 preamble
- **Then** one sentence explains that schemas declare their companion skills in a `skills.txt` manifest inside the schema directory and that skills are sourced from https://github.com/intent-driven-dev/skills
