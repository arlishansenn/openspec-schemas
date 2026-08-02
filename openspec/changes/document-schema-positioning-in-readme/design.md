## Context

The root `README.md` is the catalog entry point for this schema collection. It currently lists schemas with inconsistent depth (only behaviour-driven and intent-driven have Activation/Validate snippets; only intent-driven states its artifact order), has a heading-level bug (`## Spec-Driven With ADR` sits outside the "Custom Schemas" section), and offers no guidance on choosing between schemas. Verified facts that the new guidance must reflect:

- `intent-driven` and `spec-driven-with-adr` have identical artifact graphs (`proposal -> specs / design -> adr -> tasks`); intent-driven adds Gherkin-style GIVEN/WHEN/THEN spec content and the largest companion skill set, so it subsumes spec-driven-with-adr.
- `behaviour-driven` is not a subset of intent-driven: intent-driven borrows only the Gherkin prose style and explicitly excludes `.feature` extraction, gherkin-lint, the Cucumber.js `acceptance-tests/` project, and the failing-acceptance-before-implementation gates.
- `event-driven` (`event-storming -> event-modeling -> specs -> design -> asyncapi -> tasks`) is domain-specific and outside the general-purpose hierarchy.
- `minimalist` (`specs -> tasks`) targets small, low-risk changes.
- The `intent-driven-template` repo (https://github.com/intent-driven-dev/intent-driven-template) is a live starter with the intent-driven schema pre-installed. Per `custom-schema-packaging` spec, it is retired only as a *skills* location — skills remain canonical at https://github.com/intent-driven-dev/skills.

## Goals / Non-Goals

**Goals:**
- Give readers a decision path: `spec-driven` (upstream default) for most projects; `intent-driven` as the most complete general-purpose schema for complex projects; specialised schemas when their enforcement or domain fits.
- Document inter-schema relationships accurately (no "behaviour-driven is a subset" claim).
- Point readers at `intent-driven-template` as the zero-install way to try the flagship schema.
- Make every catalog entry structurally identical and correctly nested.

**Non-Goals:**
- No changes to per-schema READMEs under `openspec/schemas/*/README.md`.
- No changes to schema packages, templates, or `skills.txt` manifests.
- No changes to `AGENT_INSTALL.md`.

## Decisions

- **Placement**: Put "Choosing a Schema" immediately after the intro paragraph and before "Install a Schema", so readers pick before installing. Alternative (appendix at the bottom) rejected — selection is the first question a new reader has.
- **Comparison table** with columns *Schema | Artifact flow | Choose when*, including a `spec-driven (built-in)` row so the default is visible in the same view. Prose-only comparison rejected — five-plus schemas are easier to scan tabularly, with relationship nuances kept to 2–3 sentences of prose below the table.
- **Relationship wording**: state that intent-driven *subsumes* spec-driven-with-adr, and *adopts behaviour-driven's Gherkin style but not its executable acceptance-test enforcement*. This avoids overclaiming while still positioning intent-driven as most complete general-purpose.
- **Template link scope**: link `intent-driven-template` from the Intent-Driven catalog entry (and a one-liner in Choosing a Schema) as a starter template only; skills references keep pointing at intent-driven-dev/skills to respect the existing `custom-schema-packaging` requirement about the retired skills location.
- **Section ordering**: demote `## Spec-Driven With ADR` to `###` and place it before Intent-Driven so the subset precedes the superset. Catalog order becomes: Minimalist, Event-Driven, Behaviour-Driven, Spec-Driven With ADR, Intent-Driven.
- **Uniform entry shape**: every catalog entry gets description, artifact order code block, Activation YAML, Validate command, README link — matching what behaviour-driven/intent-driven already have rather than inventing a new format.

## Risks / Trade-offs

- [README drifts from schema.yaml artifact lists over time] → Keep artifact flows in the README as short arrow strings copied from each `schema.yaml`; the existing `custom-schema-packaging` validation scenario (`openspec schema validate` sanity check) still applies at implementation time.
- ["Most complete" read as "always best"] → The Choosing section explicitly says most projects are fine on `spec-driven`, and names when behaviour-driven/event-driven beat intent-driven.
- [Template repo link rot or divergence from packaged schema] → The template README itself states it bundles a local copy of the upstream schema; the root README links it as a starter, not as the schema's source of truth.
