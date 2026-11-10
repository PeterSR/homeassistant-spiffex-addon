# spiffex agent

This app gives Home Assistant a [SPIFFE](https://spiffe.io) identity from a
[spiffex](https://github.com/PeterSR/spiffex) issuer. It runs the spiffex agent, which signs in to
your issuer with a private key that never leaves this machine, and serves the standard SPIFFE
Workload API where Home Assistant Core can reach it.

To use the identity, install the [SPIFFE integration](https://github.com/PeterSR/homeassistant-spiffe)
too. The integration works with any SPIFFE implementation; this app is the spiffex way to provide one.

## Setting up

1. **Configure the app.** On the *Configuration* tab, set:
   - **Issuer URL**: your spiffex issuer, such as `https://spiffe.example.org`.
   - **Trust domain**: the trust domain the issuer announces, such as `example.org`. The agent
     refuses an issuer that announces a different one.
   - **Client ID**: this machine's client ID, `<origin>/<member>`, such as `home-assistant/core`.
2. **Start the app.** On first start it creates a private key and prints the matching public key
   in its log, under *Admit this public key at the issuer*.
3. **Admit the key at the issuer.** Add the member to a `keys` origin in the issuer's state, for
   example:

   ```yaml
   origins:
     home-assistant:
       provider: keys
       # Limit the audiences Home Assistant may request tokens for, and keep it to JWT-SVIDs.
       audiences: ["https://api.example.org"]
       svid_types: [jwt]
       members:
         core:
           keys:
             - |
               -----BEGIN PUBLIC KEY-----
               (the key from the log)
               -----END PUBLIC KEY-----
   ```

   Home Assistant is then `spiffe://example.org/home-assistant/core`.
4. **Add the SPIFFE integration** with the address `unix:///config/.spiffe/agent.sock`.

Until the key is admitted, the agent serves the trust bundle but cannot get tokens. The
integration raises a repair issue saying so.

## Options

| Option | Required | What |
|---|---|---|
| `issuer` | yes | the issuer URL |
| `trust_domain` | yes | the trust domain the issuer must announce |
| `client_id` | yes | `<origin>/<member>` at the issuer |
| `ca_file` | no | a PEM file of CA certificates to trust for the issuer instead of the system roots, as a path inside the app, such as `/homeassistant/spiffex-ca.pem` |

## Where things are

- **The Workload API** is served at `.spiffe/agent.sock` in the Home Assistant configuration
  folder, which Core sees as `/config/.spiffe/agent.sock`. The folder and the socket are readable
  by root only.
- **The private key** is `key.pem` in the app's own data folder. It is created once and reused,
  and it is left out of backups.

## Backups and restoring

The key is deliberately not in backups, so a backup restored on another machine does not copy
Home Assistant's identity. After a restore the app creates a new key and prints it. Replace the
old key in the issuer's state with the new one.

## Security

Anything that can reach the socket gets Home Assistant's identity: every integration in Core, and
apps that map the configuration folder, such as file editors and terminals. That is no weaker
than before, since write access to the configuration folder already means control of Home
Assistant. Limit the identity at the issuer with `audiences` and `svid_types`, as in the example
above.

Report vulnerabilities privately; see
[SECURITY.md](https://github.com/PeterSR/homeassistant-spiffex-addon/blob/main/SECURITY.md).
