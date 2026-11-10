# spiffex apps for Home Assistant

A Home Assistant app (add-on) repository with one app:

- **[spiffex agent](spiffex_agent/DOCS.md)**: gives Home Assistant a [SPIFFE](https://spiffe.io)
  identity from a [spiffex](https://github.com/PeterSR/spiffex) issuer, by running the spiffex
  agent and serving the SPIFFE Workload API where Home Assistant Core can reach it.

Use it with the [SPIFFE integration](https://github.com/PeterSR/homeassistant-spiffe), which turns
that Workload API into helpers other integrations can use. The integration works with any SPIFFE
implementation, so this app is one way to provide the identity, not the only one: an app for SPIRE,
Teleport or anything else only has to serve the Workload API at a path Core can see.

## Installation

1. In Home Assistant, go to **Settings → Apps → App store**, open the menu, choose
   **Repositories**, and add `https://github.com/PeterSR/homeassistant-spiffex-addon`.
2. Install **spiffex agent** and follow its documentation.

## Development

The app's image is built from `spiffex_agent/Dockerfile` for `amd64` and `arm64` and published to
`ghcr.io/petersr/homeassistant-spiffex-agent` by the *Build* workflow when a `v*` tag is pushed.
To try it locally against a throwaway trust domain:

```bash
docker build -t spiffex-agent:dev spiffex_agent
spiffex dev -dir /tmp/sxdev -listen 127.0.0.1:8443
spiffex issuer -config /tmp/sxdev/issuer.yaml -listen 127.0.0.1:8443 &
mkdir -p /tmp/app/data /tmp/app/config
echo '{"issuer":"http://127.0.0.1:8443","trust_domain":"spiffex.test","client_id":"dev/ha"}' \
  > /tmp/app/data/options.json
docker run --rm --network host -v /tmp/app/data:/data -v /tmp/app/config:/homeassistant spiffex-agent:dev
```

Admit the printed key as member `ha` of the `dev` origin in `/tmp/sxdev/state.yaml`; the Workload
API is then at `/tmp/app/config/.spiffe/agent.sock`.

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
