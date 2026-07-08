# ikigenba skills

Agent skills for **ikispec**, the ikigenba spec-first build system. Installable
into Claude and Codex with the [`skills`](https://skills.sh) CLI:

```sh
npx skills@latest add ikigenba/skills --skill='*' --copy --yes --agent claude-code codex
```

## What's here

`ikispec` is the brand for the spec system: `project/` is the source of truth, and
code is generated from it. `ralph` is the generic executor it rides on; `ralph`
knows nothing about `ikispec`.

| Skill | Layer | What it is |
| --- | --- | --- |
| `ikispec` | spec format | The authoritative output shapes, authority boundaries, and hard invariants for the `project/` spec (product, research, design, plan). |
| `author-ikispec` | authoring | One automated pass that writes a settled goal into all four `project/` docs and mints requirement ids. |
| `create-gather-build-verify-prompts` | adapter | Generates the ralph gather → build → verify loop prompts that build from an `ikispec` spec. |
| `create-audit-prompts` | adapter | Generates the ralph audit loop that re-checks test coverage of every requirement id. |
| `ralph` | executor | Orientation map of the spec-agnostic `ralph` harness that runs the loops. |
| `grillme` | utility | Interrogates a goal one question at a time until it's settled, before writing the spec. |

## Prerequisites (external tools)

The spec workflow shells out to two tools that are **not** bundled here — install
them separately and make sure both are on `PATH`:

- **`idgen`** — the CLI that mints `R-XXXX-XXXX` requirement ids
  (`idgen -n <count> -p R`). See [ikigenba/idgen](https://github.com/ikigenba/idgen).
- **`ralph`** — the harness that drives the build and audit loops.
  See [ikigenba/ralph](https://github.com/ikigenba/ralph).

## Layout

Each skill is a directory with a `SKILL.md`. Skills that expose a first-class
Codex invocation also carry an `agents/openai.yaml` describing their interface.
