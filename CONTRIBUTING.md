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
- Dependabot cannot see the base image, which is set through `ARG BUILD_FROM`: bump it by hand
  from [home-assistant/docker-base](https://github.com/home-assistant/docker-base/releases).
- To move to a new spiffex release, change `SPIFFEX_VERSION` and both `SPIFFEX_SHA256_*` values in
  the Dockerfile together, from the release's `checksums.txt`.
- For any change a user would notice, bump `version` in `spiffex_agent/config.yaml` and add an
  entry to `spiffex_agent/CHANGELOG.md`.
- Never print the private key, and keep it out of backups.

## Releases

Bump `version` in `config.yaml` and push to `main`. The *Build* workflow publishes
`ghcr.io/petersr/homeassistant-spiffex-agent:<version>` for both architectures, unless an image
with that version already exists, and then tags the commit `v<version>`. Home Assistant reads the
version from `main` straight away, so for the few minutes the build takes, an update to it fails
until the image is there.

The first time an image is published, make the package public under the repository's *Packages*
settings, or Home Assistant cannot pull it.

By contributing, you agree that your contribution is licensed under the [MIT License](LICENSE),
and you agree to follow the [code of conduct](CODE_OF_CONDUCT.md).
