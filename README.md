# agentteams-cli

The shared AgentTeams CLI tooling for OpenCharly's AgentTeams images.

The `agentteams-cli` candy builds the upstream **`agt`** REST client from the
pinned AgentTeams v1.2.2 source and extracts the **`mc`** MinIO client from the
pinned `higress/mc` image. It is one build shared by two consumers (R3): the
`agentteams-manager` and `agentteams-worker` images both compose it.

The pinned AgentTeams source tarball is left at `/opt/agentteams-src` for the
consuming candies (the manager installs its agent trees and configs, the worker
installs the shared protocol libs); each consumer removes it after use. The `agt`
build is CGO-disabled (a thin REST client, no cgo deps), so this candy needs only
the Go toolchain.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `agentteams-cli` |
| Requires | `@github.com/opencharly/layer-golang` |
| Builds | `/usr/local/bin/agt` (the AgentTeams REST client, CGO-disabled) |
| Extracts | `/usr/local/bin/mc` (the MinIO client, from the `higress/mc` image) |
| Source | left at `/opt/agentteams-src` for consumers |
| Service / port | none |
| Environment | none |

## How to use it

Compose the layer in a box's `candy:` list:

```yaml
my-agentteams-image:
  candy:
    base: cachyos-base
    candy:
      - '@github.com/opencharly/layer-agentteams-cli:<tag>'
```

Then, inside the built image:

```bash
agt --help
mc --version
```

## Layout

- `charly.yml` — the `agentteams-cli:` candy entity: the `layer-golang` require,
  the `mc` extraction, and the `plan:` (download + build + `check:` steps).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-agentteams:agentteams`
- CLI skill: `/charly-agentteams:agentteams-cli` (the compiled-in `charly agentteams` command)
- Consumers: the `agentteams-manager` and `agentteams-worker` boxes in `opencharly/layer-agentteams`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
