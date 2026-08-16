## Context

Three repositories carry the intent-driven workflow, and they have diverged:

| Repository | Last relevant commit | State |
|---|---|---|
| `intent-driven-template` | 2026-08-16 | Schema is vanilla OpenSpec; BDD workflow extracted into the opt-in `spec-as-source` skill |
| `intent-driven-dev/skills` | 2026-08-16 | Now ships `spec-as-source`; `bdd-zone-check` removed; `acceptance-test-authoring` rewritten with `EXTRACTION.md` / `COMPOSITION.md` |
| `openspec-schemas` (this repo) | 2026-08-03 | Schema still embeds fenced Gherkin, a `format:` block, `stack:`, and acceptance enforcement |

This repository moved the *other* way on 2026-08-02 (`adopt-fenced-gherkin-behaviour-intent`), pushing BDD rules into the schema. The template has since reversed that: the schema describes the artifact workflow, and the skill describes how specs become executable tests.

Verified while preparing this change:

- The template's `spec-as-source` and `acceptance-test-authoring` skill directories are **byte-identical** to the copies now in `intent-driven-dev/skills` (`diff -rq` clean), so `skills.txt` can reference `spec-as-source` immediately.
- The artifact **dependency graph is already identical** between this repo's `intent-driven/schema.yaml` and the template's: `proposal → (specs, design)`, `adr → design`, `tasks → (specs, adr)`. Only the prose differs.
- `templates/design.md` and `templates/adr.md` are already identical to the template's; only `proposal.md`, `spec.md`, and `tasks.md` differ.

Constraint from `openspec/config.yaml`: every change touching `openspec/schemas/` must pass `openspec schema validate <schema-name>`.

## Goals / Non-Goals

**Goals:**

- Make `openspec/schemas/intent-driven/` match the template's schema package, so the starter project this repository advertises and the packaged schema describe the same workflow.
- Apply the same schema/skill separation to `behaviour-driven`, which shares the machinery being removed.
- Point both `skills.txt` manifests at skills that actually exist upstream.
- Keep the root README's schema positioning true after the change.

**Non-Goals:**

- Changing `.claude/`, `.codex/`, or `.opencode/` assets. This repository's copies were regenerated for openspec 1.6.0 (`c4ae75c`); the template's predate that and use `opsx-bulk-apply` where this repo uses `opsx-bulk-archive`.
- Vendoring skill bodies. This repository declares skills by name; the bodies live in `intent-driven-dev/skills`.
- Touching `spec-driven-with-adr`, `event-driven`, or `minimalist`.
- Adding the template's `adversarial-authoring` skill, `.opencode/agent/` definitions, `opencode.json`, or `skills-lock.json`.

## Decisions

### 1. Copy the template's `intent-driven` schema files verbatim, with two deliberate deviations

Take `schema.yaml`, `templates/proposal.md`, `templates/spec.md`, and `templates/tasks.md` byte-for-byte from the template. Deviations:

- **Keep `skills.txt`.** The template has no such file — it uses `skills-lock.json` for a single skill. `skills.txt` is this repository's packaging contract (`custom-schema-packaging`), asserted by AGENT_INSTALL.md Step 6.
- **Keep the README's "Associated Skills" section**, for the same reason, appended to the template's README text.

*Alternative considered:* hand-merge the two schemas to preserve some fenced-Gherkin guidance in the schema. Rejected — a partial merge reproduces exactly the drift this change exists to remove.

### 2. Fix the stale artifact-flow line rather than copying it

The template's README states the flow as `proposal -> specs -> design -> adr -> tasks`, but its own `schema.yaml` gives `specs` and `design` both `requires: [proposal]`. The linear line is wrong in the template. This repository's README will state `proposal -> (specs, design) -> adr -> tasks`, matching the machine-readable schema.

*Alternative considered:* copy verbatim for exactness. Rejected — it would import a documented inaccuracy, and `custom-schema-packaging` requires the root catalog's artifact flows to be correct. Worth reporting upstream.

### 3. Realign `behaviour-driven` by analogy, not by copy

The template carries no `behaviour-driven` schema, so there is nothing to copy. Apply the same transformation: remove the `format:` block, replace the fenced-Gherkin `specs` instruction with OpenSpec Markdown delta headers, replace the stack-driven `tasks` template with a generic one, and drop `stack:` from activation.

This narrows what distinguishes `behaviour-driven` from the built-in `spec-driven`: the artifact graph (`specs` and `design` in parallel), the Gherkin-style scenario phrasing, and the companion skill set it installs. That is a real thinning of the schema's identity and is called out as a trade-off below.

*Alternative considered:* leave `behaviour-driven` untouched. Rejected by explicit decision — it would leave two sibling schemas with incompatible spec formats and make the root README's "intent-driven is behaviour-driven plus ADRs" framing false.

### 4. `skills.txt`: drop `bdd-zone-check`, add `spec-as-source`

`spec-as-source` absorbed `bdd-zone-check`'s zone rules and declares `gherkin-authoring` and `acceptance-test-authoring` as required sub-skills, so both stay listed.

- `intent-driven`: `acceptance-test-authoring`, `architectural-decision-records`, `c4-diagrams`, `gherkin-authoring`, `glossary`, `grill-me`, `openspec-git-discipline`, `spec-as-source`
- `behaviour-driven`: `acceptance-test-authoring`, `gherkin-authoring`, `glossary`, `openspec-git-discipline`, `spec-as-source`

### 5. Root README states acceptance enforcement as skill-provided

The "Choosing a Schema" table and the two catalog entries currently describe fenced Gherkin and acceptance suites as properties of the schemas. Restate them as properties of the `spec-as-source` skill that both schemas install, keeping the `intent-driven` = `behaviour-driven` + ADR relationship intact.

## Risks / Trade-offs

- **Breaking change for downstream projects** → Projects that authored fenced-Gherkin specs against the current schema will find the new `specs` instruction and `templates/spec.md` describe a different format. Mitigation: the `spec-as-source` skill's `references/spec.md` preserves fenced Gherkin (headings outside the fence, Given/When/Then steps inside), so adopting the skill preserves the workflow. The proposal marks this **BREAKING** and the schema README will name the skill as the migration path.
- **`behaviour-driven` loses distinctiveness** → After the change it differs from built-in `spec-driven` mainly by its parallel `specs`/`design` graph and its skill manifest. Mitigation: its README must lead with the skill pairing. If that proves too thin, retiring `behaviour-driven` in favour of `spec-driven` + `spec-as-source` is a follow-up worth considering — out of scope here.
- **Spec format is now skill-owned and schema-invisible** → Nothing in `schema.yaml` tells tooling that fences are executable, because the `format:` block is gone. Mitigation: the new format nests Gherkin fences *inside* standard `### Requirement:` / `#### Scenario:` headings, so default OpenSpec parsing still finds requirements and scenarios; the fences are scenario bodies. This is why the template needs no `format:` block.
- **Three-repo drift can recur** → This change fixes today's divergence but not the cause. Mitigation: out of scope, but worth a follow-up on which repository is canonical for the schema.
- **Existing archived changes reference the old format** → Files under `openspec/changes/archive/` document the fenced-Gherkin decisions. Mitigation: archives are historical records and are left untouched.

## Migration Plan

1. Update `intent-driven` (schema, templates, README, `skills.txt`); run `openspec schema validate intent-driven`.
2. Update `behaviour-driven` the same way; run `openspec schema validate behaviour-driven`.
3. Update the root `README.md` positioning.
4. Run `openspec validate sync-schemas-with-intent-driven-template --type change --strict`.
5. Re-run `openspec schema validate` for all five packaged schemas as a regression check.

Rollback: revert the commit; the schema folders are self-contained and no consumer state lives in this repository.

## Open Questions

- Should the template's stale linear artifact-flow line be reported upstream as a bug? (Decision 2 assumes yes, but this change does not act on the template.)
- Is `behaviour-driven` still worth packaging once the BDD workflow is skill-owned (Risk 2)? Deliberately deferred.
