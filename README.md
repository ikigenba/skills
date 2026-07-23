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

The authoring session is three spoken moves: **`open-spec`** (open the session,
scoped to `project/*`, and describe desired outcomes) → **`grill-me`**
(interrogate until every unknown is settled) → **`seal-spec`** (write the
settled goal into the spec, ready for the next `ralph` run).

| Skill | Layer | What it is |
| --- | --- | --- |
| `ikispec` | spec format | The authoritative output shapes, authority boundaries, and hard invariants for the `project/` spec (product, research, design, plan). Loaded by the authoring skills; not spoken directly. |
| `open-spec` | authoring | Opens a spec-authoring session: scope limited to `project/*`, docs-only, discussion of desired outcomes. |
| `grill-me` | authoring | Interrogates a goal one question at a time until it's settled, before writing the spec. |
| `seal-spec` | authoring | One automated pass that writes a settled goal into all four `project/` docs and mints requirement ids. |
| `create-gather-build-verify-prompts` | adapter | Generates the ralph gather → build → verify loop prompts that build from an `ikispec` spec. |
| `create-audit-prompts` | adapter | Generates the ralph audit loop that re-checks test coverage of every requirement id. |
| `ralph` | executor | Orientation map of the spec-agnostic `ralph` harness that runs the loops. |
| `doctor` | setup | Checks whether the ikigenba tool binaries are on `PATH` and installs any that are missing, on request. |

## Prerequisites (external tools)

The spec workflow shells out to tools that are **not** bundled here and must be
on `PATH`. The **`doctor`** skill checks which are present and installs the
missing ones on request — ask an agent to "run the doctor" once the skills are
installed, or run its checker directly:

```sh
sh doctor/doctor.sh            # report what's installed / missing
sh doctor/doctor.sh --install  # install the missing ones (asks per tool)
```

It manages the whole ikigenba suite — `embed`, `autotune`, `oauth`, `idgen`,
`agentrepl`, `ralph` — each a prebuilt binary released under
`github.com/ikigenba/<tool>`. The two the spec workflow leans on directly:

- **`idgen`** — the CLI that mints `R-XXXX-XXXX` requirement ids
  (`idgen -n <count> -p R`). See [ikigenba/idgen](https://github.com/ikigenba/idgen).
- **`ralph`** — the harness that drives the build and audit loops.
  See [ikigenba/ralph](https://github.com/ikigenba/ralph).

## Layout

Each skill is a directory with a `SKILL.md`. Skills that expose a first-class
Codex invocation also carry an `agents/openai.yaml` describing their interface.

## Supply-chain scanner notes

Automated scanners (e.g. Socket, run by the `skills` installer) flag two skills
as anomalies. Both are **known and accepted**, and neither is a detection of a
malicious payload:

- **`seal-spec`** (Socket MEDIUM) and **`create-audit-prompts`** (Socket LOW)
  are flagged for describing autonomous command execution, naming external
  binaries (`idgen`, `git worktree`, `mktemp`), and depending on the sibling
  `ikispec` skill ("transitive trust").

These are inherent, intended properties of the skills, not vulnerabilities:
`idgen` and `ralph` are this project's own tools (see Prerequisites above),
`ikispec` is the sibling skill in this same repo, and the `git worktree` /
mutation-testing mechanics in `create-audit-prompts` are legitimate audit
mechanics run in a scratch worktree that is always torn down. The scanners grade
both LOW/MEDIUM with the malicious-payload finding explicitly *unchecked*. There
is no pinned version to attest for `idgen`: it is built from its own spec, so its
provenance is the linked source repo, not a published package.
