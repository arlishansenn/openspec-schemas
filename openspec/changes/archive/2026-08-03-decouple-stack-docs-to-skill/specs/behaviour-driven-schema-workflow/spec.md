# behaviour-driven-schema-workflow Delta

## MODIFIED Requirements

### Requirement: Behaviour-driven README SHALL document the fenced-Gherkin workflow
The `behaviour-driven` README MUST describe the spec-as-source workflow: the two rules, the fenced-Gherkin spec format, the artifact flow, and activation including the `stack:` key and the schema-name spelling note. Stack details (runners, reports, lint setup) are owned by the `acceptance-test-authoring` skill; the README delegates them to the skill's documentation in the skills repository rather than duplicating them.

Affected schema:
- `behaviour-driven` (`openspec/schemas/behaviour-driven/`)

#### Scenario: README documents format, flow, and stacks
- **WHEN** a contributor reads the schema README
- **THEN** it documents the two rules (acceptance tests always pass; specs and code never modified together, `tasks.md` exempt)
- **AND** it documents the fenced-Gherkin spec format with a short example and points to the `format:` block in `schema.yaml` as the machine-readable definition
- **AND** it shows the artifact flow `proposal -> (specs, design) -> tasks`
- **AND** it states that the acceptance suite is provided by the `acceptance-test-authoring` skill, which requires a `stack:` key in `openspec/config.yaml`, and links to the skill's documentation at https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/acceptance-test-authoring for the supported stacks and setup details
- **AND** it does not duplicate the skill's stack documentation (runner, report path, and lint configuration tables).

#### Scenario: README documents activation with the stack key
- **WHEN** a contributor follows the README's activation guidance
- **THEN** it tells them to set `schema: behaviour-driven` and `stack: javascript` or `stack: python` in `openspec/config.yaml`
- **AND** it notes that the acceptance-test-authoring skill's upstream docs show `schema: behavior-driven` (American spelling) while this package uses `behaviour-driven`.
