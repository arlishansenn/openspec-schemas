# Proposal: document-skills-in-root-readme

## Why

The install-associated-skills change gave every schema a `skills.txt` manifest and added Step 6 to `AGENT_INSTALL.md`, which installs companion skills into `.agents/skills/`. The per-schema READMEs document this, but the repository root `README.md` still presents installation as schema-copy only, so readers browsing the repo have no hint that installing a schema also brings in skills.

## What Changes

- Add a short paragraph to the root `README.md` "Install a Schema" section noting that each schema declares companion skills in a `skills.txt` manifest, and that the agent install guide (Step 6 of `AGENT_INSTALL.md`) installs them from https://github.com/intent-driven-dev/skills into `.agents/skills/`.
- Extend the `custom-schema-packaging` "Repository root README" requirement so the root README must mention associated skills and where they come from.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `custom-schema-packaging`: the "Repository root README" requirement additionally requires the root README to note that schemas declare companion skills in `skills.txt` and that the install guide's skills step installs them into `.agents/skills/` from the canonical skills repository.

## Impact

- `README.md` (root) — new paragraph in the "Install a Schema" section.
- `openspec/specs/custom-schema-packaging/spec.md` — requirement text updated at archive/sync time via the delta spec.
- No schema directories change, so no `openspec schema validate <schema>` runs are strictly required; validation will still be run as a sanity check.
