# Behaviour-Driven OpenSpec Schema

`behaviour-driven` is a spec-as-source BDD workflow: business use cases are
written as Gherkin scenarios inside Markdown specs, and those scenarios run as
the acceptance suite that every change must keep green. Directly updating code
without first updating the spec is forbidden — every change starts as a spec
delta whose red scenarios define the work.

## The Two Rules

1. **Acceptance tests must always pass.** Run the suite after every code
   change; never leave it red. If failing code was written without a driving
   spec delta, it is reverted and redone spec-first — never patched into
   passing.
2. **Specs and code are never modified together.** A unit of work touches
   either `openspec/` (the specs zone) or application code, never both.
   `tasks.md` files are exempt. The `bdd-zone-check` skill documents the
   discipline and a reference pre-edit hook that enforces it.

## Spec Format

A spec is a standard Markdown `spec.md`: prose (context, rationale, links) may
appear anywhere, and ALL executable Gherkin lives inside fences opened by
` ```gherkin ` at column 0. Each file's fences concatenate into exactly one
`Feature:` (the capability). `Rule:` is one requirement, described with
SHALL/MUST; `Scenario:` is a concrete Given/When/Then example, and every
`Rule:` has at least one. Delta operations are `# @openspec:` comments inside
a fence, immediately above the `Rule:` they apply to:

````markdown
# User authentication changes

Login is being tightened — see proposal.md for motivation.

```gherkin
Feature: User authentication changes

  # @openspec: ADDED
  Rule: Email must be verified before login
    Unverified accounts MUST NOT be able to log in to the system.

    Scenario: Unverified user is blocked
      Given an account with an unverified email address
      When the user attempts to log in
      Then access is denied
```
````

The machine-readable definition of this format (fence, requirement, scenario,
and delta-marker patterns) is the `format:` block in `schema.yaml`.

## Canonical Flow

Artifact order:

```text
proposal -> (specs, design) -> tasks
```

`specs` and `design` each require only the proposal and can proceed in
parallel; `tasks` requires both. At apply time, the generated tasks first
scaffold the acceptance suite (once per project), then work one pending step
definition at a time — fails for the right reason → implement → passes →
commit — until the full suite is green with zero pending or undefined steps.

## Acceptance Stacks

The acceptance suite — extraction, runners, reports, and linting — is defined
by the [`acceptance-test-authoring`](https://github.com/intent-driven-dev/skills/tree/main/.agents/skills/acceptance-test-authoring)
skill, which requires a `stack:` key in `openspec/config.yaml`. Read the
skill's documentation for the supported stacks and setup details.

## Activate

Set this in `openspec/config.yaml`:

```yaml
schema: behaviour-driven
stack: javascript # or python
```

Note on spelling: the `acceptance-test-authoring` skill's upstream docs show
`schema: behavior-driven` (American spelling, from the behavior-driven-template
repo). In this package the schema name is `behaviour-driven` — use the British
spelling in `config.yaml`.

## Validate

```bash
openspec schema validate behaviour-driven
```

## Associated Skills

This schema declares its companion skills in `skills.txt`; they are installed automatically by Step 6 of `AGENT_INSTALL.md` into `.agents/skills/`, sourced from [intent-driven-dev/skills](https://github.com/intent-driven-dev/skills).

- `acceptance-test-authoring` — the acceptance-suite contract: Gherkin extraction from `spec.md` fences, effective-spec composition (source of truth + active deltas), runner setup for both stacks, linting, and reports.
- `bdd-zone-check` — spec-first discipline and specs/code zone isolation, with a reference enforcement hook.
- `gherkin-authoring` — writing and reviewing Gherkin/BDD scenarios.
- `glossary` — keeping domain/technical terms consistent across artifacts.
- `openspec-git-discipline` — git hygiene for OpenSpec propose/apply/archive workflows.
