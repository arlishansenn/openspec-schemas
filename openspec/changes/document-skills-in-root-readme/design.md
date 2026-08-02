# Design: document-skills-in-root-readme

## Context

The install-associated-skills change added `skills.txt` manifests to all five schemas and Step 6 to `AGENT_INSTALL.md`, which installs each schema's companion skills from https://github.com/intent-driven-dev/skills into the target project's `.agents/skills/`. Per-schema READMEs got "Associated Skills" sections, but the root `README.md` "Install a Schema" section still describes the install as schema-copy only.

## Goals / Non-Goals

**Goals:**
- The root README tells readers, before they start an install, that installing a schema also installs its associated skills, where they are declared (`skills.txt`), where they come from (intent-driven-dev/skills), and where they land (`.agents/skills/`).

**Non-Goals:**
- No changes to `AGENT_INSTALL.md`, schema directories, `skills.txt` manifests, or per-schema READMEs.
- No enumeration of individual skills in the root README — that stays in each schema's README.

## Decisions

- **Placement**: one short paragraph in the "Install a Schema" section, after the schema-selection prompts (README line 27) and before the `config.yaml` example. Readers see it in the same breath as the install prompt, and the section already links to `AGENT_INSTALL.md` for details. Alternative considered: a dedicated "Associated Skills" top-level section — rejected as disproportionate for one paragraph and it would drift from the install context.
- **Wording**: mirror `AGENT_INSTALL.md` line 3 ("Schemas declare their companion skills in a `skills.txt` manifest inside the schema directory; those skills are sourced from … and installed in Step 6") so the two documents cannot disagree.
- **Spec delta**: extend the existing "Repository root SHALL provide catalog and install guidance" requirement via MODIFIED (full block copied) rather than ADDED, since this changes what the root README must contain.

## Risks / Trade-offs

- [Docs drift if Step 6 mechanics change later] → the root README paragraph names only stable facts (manifest name, source repo, destination dir) and defers mechanics to `AGENT_INSTALL.md`.
