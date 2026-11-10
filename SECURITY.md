# Security policy

This app holds Home Assistant's private key and hands out its identity, so security reports get
priority.

## Reporting a vulnerability

Please report vulnerabilities **privately** through GitHub's private vulnerability reporting: the
**Security** tab of
[PeterSR/homeassistant-spiffex-addon](https://github.com/PeterSR/homeassistant-spiffex-addon), then
*Report a vulnerability*. If that form is unavailable, use the contact options on the maintainer's
[GitHub profile](https://github.com/PeterSR) to arrange a private channel. Do not open a public
issue for anything you believe is a security problem.

You can expect an acknowledgement within a few days.

## Scope

In scope:

- the private key: how it is created, its permissions, and keeping it out of logs and backups;
- the socket directory's permissions;
- the image: how the spiffex binary is fetched and verified.

Not vulnerabilities of this app, by design:

- anything that can reach the socket, including every integration in Home Assistant Core and apps
  that map the configuration folder, obtaining Home Assistant's identity. Limit what the identity
  may do at the issuer.
- vulnerabilities in spiffex itself. Report those to
  [spiffex](https://github.com/PeterSR/spiffex/security).

## Supported versions

Before 1.0, security fixes target the latest release only.
