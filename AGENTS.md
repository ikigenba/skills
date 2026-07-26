# ikigenba skills

The agent skills for **ikispec**, the ikigenba spec-first build system, published
as a skills bundle installable into Claude and Codex. The repo is documentation
only: each skill is a directory holding a `SKILL.md` whose prose *is* the
deliverable — the instructions an agent follows when the skill is loaded. See
`README.md` for what each skill does and how to install them.

## How changes are made

Changes are direct edits to the skill files, made only on explicit operator
direction — there is no `project/` spec and no build loop here. Keep an edit
scoped to the skill it belongs to; a wording change in one `SKILL.md` should not
drag its siblings along, and cross-skill facts stay stated once in the skill
that owns them (`ikispec` owns the spec shapes, `ralph` owns the harness).

One caveat: this repo defines org-wide workflow law. The `$ikispec` invariants
and the `$open-spec` → `$grill-me` → `$seal-spec` workflows are read by agents
in every consuming project, so rewording policy text here changes agent behavior
everywhere. Propose the new wording and get it confirmed before writing it.

## Layout

- `ikispec/` — spec format: authoritative `project/` output shapes, authority
  boundaries, hard invariants. Loaded by the authoring skills, not spoken.
- `open-spec/`, `grill-me/`, `seal-spec/` — the three authoring moves that open
  a spec session, interrogate a goal until settled, and write it into `project/`.
- `create-gather-build-verify-prompts/`, `create-audit-prompts/` — adapters that
  generate the ralph loop prompts which build from and audit an ikispec spec.
- `ralph/` — orientation map of the spec-agnostic `ralph` executor.
- `doctor/` — checks the ikigenba tool binaries on `PATH` and installs missing
  ones; carries `doctor.sh`, the only executable in the repo.

Every skill directory has a `SKILL.md` with YAML front matter (`name`,
`description`) followed by the instruction body. Skills that expose a
first-class Codex invocation also carry `agents/openai.yaml` declaring their
display name, default prompt, and implicit-invocation policy.
