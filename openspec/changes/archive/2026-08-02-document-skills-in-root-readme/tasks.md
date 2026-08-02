# Tasks: document-skills-in-root-readme

## 1. Root README Update

- [x] 1.1 Add a paragraph to the "Install a Schema" section of `README.md` (after the schema-selection prompts, before the `config.yaml` example) stating that each schema declares companion skills in a `skills.txt` manifest and that the install guide's Step 6 installs them into `.agents/skills/` from https://github.com/intent-driven-dev/skills, mirroring the wording of `AGENT_INSTALL.md` line 3

## 2. Verification

- [x] 2.1 Confirm the new paragraph's facts match `AGENT_INSTALL.md` Step 6 (manifest at `openspec/schemas/<name>/skills.txt`, source repo intent-driven-dev/skills, destination `.agents/skills/`) and that no files under `openspec/schemas/` changed
- [x] 2.2 Run `openspec schema validate` as a sanity check and confirm it passes
