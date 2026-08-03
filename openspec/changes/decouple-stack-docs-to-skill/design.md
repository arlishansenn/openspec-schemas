# Design: Decouple Stack Docs from the Install Guide

## Context

Yesterday's `adopt-fenced-gherkin-behaviour-intent` change introduced the `stack:` key in three places: an AGENT_INSTALL Step 4 sentence naming the two schemas, and detailed supported-stacks tables in both schema READMEs. The canonical owner of the stack contract (runners, report paths, shared gherkin-lint config, setup) is the `acceptance-test-authoring` skill in the skills repo, which is being updated to document it.

## Goals / Non-Goals

**Goals:**
- Keep `AGENT_INSTALL.md` schema-agnostic.
- Schema READMEs mention the `stack:` requirement (they are the schema-facing docs) but delegate stack details to the skill's documentation via a link.

**Non-Goals:**
- No changes to schema.yaml files, templates, or skills.txt.
- No changes to the skills repo from here (updated separately by the maintainer).

## Decisions

- **Delete, don't generalize, the AGENT_INSTALL sentence.** The schema README is where a reader lands for schema-specific configuration; a generic "check the README" sentence adds nothing the flow doesn't already imply. Alternative (keep a generic pointer) rejected as filler.
- **Keep `stack:` in the Activate snippets.** Activation genuinely fails its purpose without the key, so the snippet keeps `stack: javascript # or python`; everything beyond the key itself (runner choice, reports, lint) moves behind the link to https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/acceptance-test-authoring.
- **Spec deltas keep scenario titles unchanged** in MODIFIED blocks (archive rejects renamed/dropped scenarios, as hit yesterday).

## Risks / Trade-offs

- [Skills repo docs lag this change] → The maintainer is updating them in parallel; the READMEs' link plus the retained `stack:` snippet keep installers unblocked meanwhile.

## Migration Plan

Docs-only; propose → apply → archive on main. Rollback = git revert.

## Open Questions

None.
