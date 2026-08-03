# Tasks

## 1. Documentation edits

- [ ] 1.1 Delete the Step 4 sentence in `AGENT_INSTALL.md` that names `behaviour-driven`/`intent-driven` and their `stack:` requirement
- [ ] 1.2 In `openspec/schemas/behaviour-driven/README.md`, replace the Supported Stacks table and runner/report/lint details with a short paragraph stating the acceptance suite is provided by the `acceptance-test-authoring` skill (which requires `stack:` in `openspec/config.yaml`) and linking to https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/acceptance-test-authoring for supported stacks and setup; keep `stack:` in the Activate snippet and keep the spelling note
- [ ] 1.3 In `openspec/schemas/intent-driven/README.md`, apply the same replacement to the stacks table in the Acceptance Enforcement section; keep the two rules and `stack:` in the Activate snippet

## 2. Verification

- [ ] 2.1 Run `openspec schema validate behaviour-driven` and `openspec schema validate intent-driven`
- [ ] 2.2 Greps: no `| \`stack:\` |` table rows left in either schema README; the skill docs URL present in both; `AGENT_INSTALL.md` no longer mentions `stack`; `stack:` still present in both Activate snippets
- [ ] 2.3 Run `openspec validate decouple-stack-docs-to-skill --type change --strict`
