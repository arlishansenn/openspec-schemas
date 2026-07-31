# Tasks: install-associated-skills

## 1. Implement the Change

- [x] 1.1 Create `openspec/schemas/<name>/skills.txt` for all 5 schemas (one skill name per line, nothing else):
  - `minimalist`: openspec-git-discipline
  - `behaviour-driven`: gherkin-authoring, glossary, openspec-git-discipline
  - `intent-driven`: architectural-decision-records, gherkin-authoring, c4-diagrams, glossary, grill-me, openspec-git-discipline
  - `spec-driven-with-adr`: architectural-decision-records, openspec-git-discipline
  - `event-driven`: c4-diagrams, glossary, openspec-git-discipline
- [x] 1.2 Append "Step 6 — Install Associated Skills" to `AGENT_INSTALL.md` after Step 5, in the guide's existing style: manifest check (Option A/B paths, skip if absent), `--depth 1` clone of https://github.com/intent-driven-dev/skills to a temp dir (handle pre-existing dir), per-skill copy into `./.agents/skills/`, ask-before-overwrite for existing skills, report-and-continue for skills missing from the clone, final installed-skills summary line.
- [x] 1.3 Add one sentence to the `AGENT_INSTALL.md` intro (or Step 6 preamble) explaining that schemas declare companion skills in `skills.txt`, sourced from https://github.com/intent-driven-dev/skills.
- [x] 1.4 In `openspec/schemas/spec-driven-with-adr/README.md`: repoint the ADR skills link to https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/architectural-decision-records and drop the `## Pending` "Package schema and associated skills together" item (fulfilled by this change).
- [x] 1.5 Add an `## Associated Skills` section to all 5 schema READMEs: list that schema's skills with one-line purposes, link the skills repo, note automatic install via `AGENT_INSTALL.md` Step 6 into `.agents/skills/`.

## 2. Verify and Wrap Up

- [x] 2.1 Fresh `--depth 1` clone of the skills repo; confirm every name in every `skills.txt` exactly matches a directory under `.agents/skills/`.
- [x] 2.2 Simulate an install in a scratch directory (AGENT_INSTALL Steps 1–3 and 6 for `intent-driven`; skip Steps 4–5 if the OpenSpec CLI is unavailable there): all 6 skills land in `.agents/skills/`; a second run triggers the already-exists prompt, not a silent overwrite.
- [x] 2.3 Delete `skills.txt` in the scratch copy and confirm Step 6 skips cleanly.
- [x] 2.4 Run `openspec schema review <name>` (unavailable in CLI 1.6.0 — ran `openspec schema validate <name>` instead; all passed) for `minimalist`, `behaviour-driven`, `intent-driven`, `spec-driven-with-adr`, and `event-driven`; all must pass before the apply is complete.
- [x] 2.5 Prepare concise reviewer notes; delete `SKILLS_INSTALL_HANDOFF.md` (unstage it first — do not commit it).
