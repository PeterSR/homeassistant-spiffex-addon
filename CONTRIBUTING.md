# Contributing

Thanks for considering a contribution. This app aims to stay a thin, careful wrapper around the
spiffex agent.

## Layout

| Path | What |
|---|---|
| `repository.yaml` | the app repository's name and maintainer |
| `spiffex_agent/config.yaml` | the app's configuration, options and schema |
| `spiffex_agent/Dockerfile` | the image: the Home Assistant base image plus a pinned, checksummed spiffex release |
| `spiffex_agent/rootfs/etc/s6-overlay/` | the s6 service that runs the agent, and stops the container when it fails |
| `spiffex_agent/DOCS.md` | the documentation shown in Home Assistant |

## Changes

- Run `shellcheck` on the scripts in `spiffex_agent/rootfs/etc/s6-overlay/s6-rc.d/spiffex-agent/`.
- To move to a new spiffex release, change `SPIFFEX_VERSION` and both `SPIFFEX_SHA256_*` values in
  the Dockerfile together, from the release's `checksums.txt`.
- For any change a user would notice, bump `version` in `spiffex_agent/config.yaml` and add an
  entry to `spiffex_agent/CHANGELOG.md`.
- Never print the private key, and keep it out of backups.

## Releases

Push a tag `vX.Y.Z` matching `version` in `config.yaml`. The *Build* workflow publishes the image
for both architectures, and Home Assistant offers the update.

By contributing, you agree that your contribution is licensed under the [MIT License](LICENSE),
and you agree to follow the [code of conduct](CODE_OF_CONDUCT.md).
