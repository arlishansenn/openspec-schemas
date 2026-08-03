# Decouple Stack Docs from the Install Guide, Delegate to the Skills Repo

## Why

The `stack:` config key belongs to the `acceptance-test-authoring` skill — it selects which acceptance-suite reference runner the skill scaffolds. `AGENT_INSTALL.md` naming specific schemas and their `stack:` values couples the generic install flow to one skill's requirement, and the schema READMEs duplicating the supported-stacks table (runners, report paths, lint config) duplicates documentation that canonically lives with the skill. The skills repo is being updated to document this; this repo should link there instead.

## What Changes

- `AGENT_INSTALL.md` Step 4 drops the sentence naming `behaviour-driven`/`intent-driven` and their `stack:` requirement — the install guide stays schema-agnostic.
- `openspec/schemas/behaviour-driven/README.md` replaces its Supported Stacks table with a short paragraph: the acceptance suite is defined by the `acceptance-test-authoring` skill, which requires `stack:` in `openspec/config.yaml`; supported stacks and setup are documented in the skill's docs in the skills repo (linked). The `stack:` line stays in the Activate snippet.
- `openspec/schemas/intent-driven/README.md` gets the same treatment for the stacks table in its Acceptance Enforcement section.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `behaviour-driven-schema-workflow`: the README requirement's stacks clause changes from "lists the supported stacks (runners, reports, shared lint config)" to "states the acceptance suite is provided by the acceptance-test-authoring skill, which requires `stack:` in `openspec/config.yaml`, and links to the skill's documentation for supported stacks and setup".
- `intent-driven-schema-workflow`: same substitution in its README requirement's "documents … the supported stacks" clause.

## Impact

- `AGENT_INSTALL.md`, `openspec/schemas/behaviour-driven/README.md`, `openspec/schemas/intent-driven/README.md` — docs only; no schema.yaml, template, or skills.txt changes.
- Depends on the skills repo documenting the `stack:` requirement in `acceptance-test-authoring` (being updated separately).
