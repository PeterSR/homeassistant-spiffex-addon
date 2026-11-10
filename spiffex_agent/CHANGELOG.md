# Changelog

## 0.1.2

- Ship spiffex's Apache-2.0 licence in the image, and label the image with both licences.
- Label the image with the commit it was built from.

## 0.1.1

- Write the private key atomically, so a power cut on first start cannot leave a half-written key.
- If the key cannot be read, set it aside, create a new one, and say so in the log.
- Show the app's own message when a required option is empty.

## 0.1.0

- First version: runs spiffex agent 0.4.1, creates this machine's key on first start, and serves
  the SPIFFE Workload API at `/config/.spiffe/agent.sock` for Home Assistant Core.
