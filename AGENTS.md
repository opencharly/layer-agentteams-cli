# AGENTS.md — layer-agentteams-cli

Standalone candy repo for the `agentteams-cli` layer. The whole candy lives in
`charly.yml` at the repo root: a `layer-golang` require, an `extract:` of the
`mc` MinIO client from the pinned `higress/mc` image, and an ordered `plan:` that
downloads and builds the upstream `agt` REST client from the pinned AgentTeams
v1.2.2 source. There is no source tree and no runtime service.

**No dedicated owning skill exists for this candy.** The `agt` binary is not (yet)
covered by a skill in the marketplace corpus; the closest procedures are the
family stack skill and the compiled-in CLI skill, both listed below. If a future
change adds a `skill:` entity, project it as `/charly-agentteams:<name>` here.

Canonical files:

- `charly.yml` — the `agentteams-cli:` candy entity (require, extract, plan).
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; there is no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-agentteams:agentteams` — the family stack skill: how the manager and
  worker images that consume this candy are composed and deployed. Load before
  editing or troubleshooting the candy.
- `/charly-agentteams:agentteams-cli` — the compiled-in `charly agentteams` REST
  CLI (a different surface from this candy's upstream `agt` binary; useful for
  understanding the controller API the binaries talk to).
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`run:`/`check:`, `extract:`). Load before
  editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the same structural gate CI runs.
  Keep the `version:` schema stamp within the installed charly's supported range.
- The merge gate is the org-wide `charly/pr-validator` (required check
  `validate / validate`); there is no per-repo candy gate.
- There is no live bed: the candy is a build, so the evidence is its `plan:`
  `check:` steps — `/usr/local/bin/agt` is a file and `/usr/local/bin/mc` is a
  file.

## Modify this repo

- There is no embedded `skill:` entity to mirror here; a package or build change
  is described by the candy `description:` and proven by the `plan:` `check:`
  steps. If a dedicated skill is added later, add it as a `skill:` entity in the
  same change.
- The pinned source tag and the `mc` image tag are the production contract; bump
  them together with any change to the build commands.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
