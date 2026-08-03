# intent-driven-schema-workflow Delta

## MODIFIED Requirements

### Requirement: Intent-driven README SHALL document the acceptance-enforced workflow
The `intent-driven` README MUST position the schema as `behaviour-driven` plus durable ADRs and document the fenced-Gherkin format, acceptance enforcement, and activation including the `stack:` key. Stack details (runners, reports, lint setup) are owned by the `acceptance-test-authoring` skill; the README delegates them to the skill's documentation in the skills repository rather than duplicating them.

Affected schema:
- `intent-driven` (`openspec/schemas/intent-driven/`)

#### Scenario: README documents format, enforcement, and fit
- **WHEN** a contributor reads `openspec/schemas/intent-driven/README.md`
- **THEN** it describes the schema as `behaviour-driven` plus per-change ADR review and durable ADR records
- **AND** it documents the fenced-Gherkin spec format with an example and states that Gherkin is extracted to `.feature` files at test time and linted with gherkin-lint
- **AND** it documents the two rules
- **AND** it states that the acceptance suite is provided by the `acceptance-test-authoring` skill, which requires a `stack:` key in `openspec/config.yaml`, and links to the skill's documentation at https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/acceptance-test-authoring for the supported stacks and setup details, without duplicating the skill's stack documentation
- **AND** it shows the artifact flow `proposal -> (specs, design) -> adr -> tasks`
- **AND** activation guidance covers `schema: intent-driven` and `stack: javascript` or `stack: python`
- **AND** it no longer claims that `.feature` files must not be created or that Gherkin linting is outside the schema.
