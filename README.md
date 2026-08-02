# OpenSpec Custom Schemas

Custom [OpenSpec](https://github.com/Fission-AI/OpenSpec) schemas packaged as copyable folders under `openspec/schemas/`.

Default OpenSpec includes the `spec-driven` schema, which is a strong general-purpose workflow. This repo adds more focused workflows for specific delivery contexts, and also demonstrates how to customise OpenSpec for different styles of work.

Detailed write-up: [https://intent-driven.dev/blog/2026/02/12/openspec-custom-schemas/](https://intent-driven.dev/blog/2026/02/12/openspec-custom-schemas/)

## Video

[![Watch on YouTube](https://img.youtube.com/vi/k01nbZfwB34/0.jpg)](https://www.youtube.com/watch?v=k01nbZfwB34)

## Choosing a Schema

For most projects, the built-in `spec-driven` schema is all you need. For complex projects — meaningful behaviour, technical design, and long-lived architectural decisions — `intent-driven` is the most complete general-purpose schema in this collection. The remaining schemas are either lighter subsets or specialised for a particular style of delivery.

| Schema | Artifact flow | Choose when |
|--------|---------------|-------------|
| `spec-driven` (built-in) | `proposal -> specs -> design -> tasks` | Default for most projects; ships with OpenSpec |
| `minimalist` | `specs -> tasks` | Small, well-scoped, low-risk changes |
| `behaviour-driven` | `proposal -> (specs, design) -> tasks` | You want executable BDD: `.feature` files and Cucumber acceptance gates driving implementation |
| `spec-driven-with-adr` | `proposal -> specs / design -> adr -> tasks` | You need durable Architecture Decision Records on top of spec-driven |
| `intent-driven` | `proposal -> specs / design -> adr -> tasks` | Complex changes needing Gherkin-style behaviour specs, design, and durable ADRs |
| `event-driven` | `event-storming -> event-modeling -> specs -> design -> asyncapi -> tasks` | Event-Driven Architecture Systems |

How the schemas relate: `intent-driven` subsumes `spec-driven-with-adr` — same artifact graph, plus Gherkin-style `GIVEN` / `WHEN` / `THEN` specs and a larger companion skill set. It adopts `behaviour-driven`'s Gherkin *style* but intentionally not its executable acceptance-test enforcement, so `behaviour-driven` remains the right choice when you want `.feature` files and Cucumber gates. `event-driven` is domain-specific for event-centric/AsyncAPI-first systems, and `minimalist` is for small, low-risk changes.

To try `intent-driven` without installing anything, start from the [intent-driven-template](https://github.com/intent-driven-dev/intent-driven-template) — a starter project with the schema, OpenSpec config, commands, and companion skills already installed.

## Install a Schema

Ask your coding agent to read the install guide and follow the instructions:

```text
Read this file: https://raw.githubusercontent.com/intent-driven-dev/openspec-schemas/refs/heads/main/AGENT_INSTALL.md and follow the instructions.
```

If you already know which schema you want, include the name and the guide will confirm it exists before proceeding:

```text
Read this file: https://raw.githubusercontent.com/intent-driven-dev/openspec-schemas/refs/heads/main/AGENT_INSTALL.md and install schema intent-driven.
```

Otherwise the guide will enumerate all available schemas and ask you to pick one.

Schemas declare their companion skills in a `skills.txt` manifest inside the schema directory. The install guide's Step 6 installs those skills from [intent-driven-dev/skills](https://github.com/intent-driven-dev/skills) into your project's `.agents/skills/`, so installing a schema also brings in the skills it works best with.

### Example: intent-driven `config.yaml`

```yaml
schema: intent-driven

context: |
  Tech Stack:
    - Node.js, TypeScript
    - PostgreSQL

rules:
  proposal:
    - Maximum of 250 words
  tasks:
    - Break tasks to logical commits.
```

Artifact alignment source: `openspec/schemas/intent-driven/schema.yaml` (`proposal`, `specs`, `design`, `adr`, `tasks`).

For the full step-by-step install flow, see [`AGENT_INSTALL.md`](./AGENT_INSTALL.md).

## Custom Schemas

### Minimalist

Fast path from spec to execution using user-story requirements and Gherkin acceptance-criteria style. Lightweight schema for well-scoped, low-risk changes.

Artifact order:

```text
specs -> tasks
```

Activation:

```yaml
schema: minimalist
```

Validate:

```bash
openspec schema validate minimalist
```

For more details, see `openspec/schemas/minimalist/README.md`.

### Event-Driven

Structured workflow for event-centric systems with [Event Storming](https://en.wikipedia.org/wiki/Event_storming) discovery followed by [AsyncAPI](https://www.asyncapi.com/) specification.

Artifact order:

```text
event-storming -> event-modeling -> specs -> design -> asyncapi -> tasks
```

Activation:

```yaml
schema: event-driven
```

Validate:

```bash
openspec schema validate event-driven
```

For more details, see `openspec/schemas/event-driven/README.md`.

### Behaviour-Driven

Proposal-led workflow for changes where observable behaviour should drive design
and implementation. Specs stay mergeable by OpenSpec as `spec.md` files, while
the requirement and scenario content uses Gherkin-style `GIVEN` / `WHEN` /
`THEN` steps. At apply time, specs are extracted into `.feature` files backed by
failing Cucumber acceptance tests that must pass before the change completes —
executable enforcement that `intent-driven` intentionally does not include.

Artifact order:

```text
proposal -> (specs, design) -> tasks
```

Activation:

```yaml
schema: behaviour-driven
```

Validate:

```bash
openspec schema validate behaviour-driven
```

For more details, see `openspec/schemas/behaviour-driven/README.md`.

### Spec-Driven With ADR

Experimental proposal-to-tasks workflow for changes that also need durable
Architecture Decision Records persisted under the target repository's top-level
`adr/` folder. `intent-driven` shares this schema's artifact graph and adds
Gherkin-style specs plus a larger skill set — prefer it unless you want plain
spec-driven specs with ADRs and nothing more.

Artifact order:

```text
proposal -> specs / design -> adr -> tasks
```

Activation:

```yaml
schema: spec-driven-with-adr
```

Validate:

```bash
openspec schema validate spec-driven-with-adr
```

For more details, see `openspec/schemas/spec-driven-with-adr/README.md`.

### Intent-Driven

Proposal-led workflow for changes that need observable behaviour, technical
design, durable Architecture Decision Records, and implementation tasks. Specs
stay mergeable by OpenSpec as `spec.md` files, while the requirement and
scenario content uses Gherkin-style `GIVEN` / `WHEN` / `THEN` steps.

To try it without installing anything, start from the
[intent-driven-template](https://github.com/intent-driven-dev/intent-driven-template) —
a starter project with the schema, OpenSpec config, commands, and companion
skills already installed. (Companion skills are canonically hosted at
[intent-driven-dev/skills](https://github.com/intent-driven-dev/skills).)

Artifact order:

```text
proposal -> specs -> design -> adr -> tasks
```

Activation:

```yaml
schema: intent-driven
```

Validate:

```bash
openspec schema validate intent-driven
```

For more details, see `openspec/schemas/intent-driven/README.md`.

## Contributing

See `CONTRIBUTING.md` for how to create/customize schemas using `openspec schema init` / `openspec schema fork`, and how to validate before opening a PR.
