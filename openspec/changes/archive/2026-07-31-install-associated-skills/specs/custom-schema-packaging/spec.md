## ADDED User Stories

### User Story: Schemas declare companion skills in a manifest
As a schema maintainer, I want each schema to declare its companion skills in a `skills.txt` manifest inside the schema folder, so that the manifest travels with the schema when it is copied and the installer can act on it.

Affected schemas: `minimalist`, `behaviour-driven`, `intent-driven`, `spec-driven-with-adr`, `event-driven`.

#### Acceptance Criteria
- **Given** the repository's `openspec/schemas/` directory
- **When** I inspect any of the five schema folders
- **Then** each contains a `skills.txt` with one skill name per line and nothing else:
  - `minimalist`: `openspec-git-discipline`
  - `behaviour-driven`: `gherkin-authoring`, `glossary`, `openspec-git-discipline`
  - `intent-driven`: `architectural-decision-records`, `gherkin-authoring`, `c4-diagrams`, `glossary`, `grill-me`, `openspec-git-discipline`
  - `spec-driven-with-adr`: `architectural-decision-records`, `openspec-git-discipline`
  - `event-driven`: `c4-diagrams`, `glossary`, `openspec-git-discipline`
- **And** every listed name exactly matches a directory under `.agents/skills/` in a fresh clone of https://github.com/intent-driven-dev/skills

### User Story: Schema READMEs document associated skills
As a user evaluating a schema, I want its README to list the associated skills with a one-line purpose each, so that I know what companion tooling the schema installs and why.

#### Acceptance Criteria
- **Given** any of the five schema READMEs under `openspec/schemas/<name>/README.md`
- **When** I read its "Associated Skills" section
- **Then** it lists exactly the skills from that schema's `skills.txt`, each with a one-line purpose
- **And** it links to https://github.com/intent-driven-dev/skills
- **And** it notes the skills are installed automatically by Step 6 of `AGENT_INSTALL.md` into `.agents/skills/`

## MODIFIED User Stories

### User Story: spec-driven-with-adr README points at the canonical skills repo
As a reader of the `spec-driven-with-adr` README, I want the ADR skills reference to point at the canonical skills repository, so that I am not sent to the stale `intent-driven-template` location.

#### Acceptance Criteria
- **Given** `openspec/schemas/spec-driven-with-adr/README.md`
- **When** I follow the ADR skills reference
- **Then** it points to https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/architectural-decision-records
- **And** the README no longer lists "Package schema and associated skills together" as pending, since packaging is fulfilled by the `skills.txt` manifest and install Step 6

## Notes

Post-apply verification: this change modifies files under `openspec/schemas/` for all five schemas, so `openspec schema review <schema-name>` MUST be run for `minimalist`, `behaviour-driven`, `intent-driven`, `spec-driven-with-adr`, and `event-driven` — and pass — before the apply is considered complete.
