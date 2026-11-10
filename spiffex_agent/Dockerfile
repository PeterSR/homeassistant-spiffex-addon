ARG BUILD_FROM=ghcr.io/home-assistant/base:3.24-2026.11.0
FROM ${BUILD_FROM}

# The spiffex release this app runs. Bump the version and both checksums together, from the
# release's checksums.txt.
ARG SPIFFEX_VERSION=0.4.1
ARG SPIFFEX_SHA256_amd64=b7503527fbd95bb28545bf6c69f79e68d2648b70185c3620f7c6f45becb05128
ARG SPIFFEX_SHA256_arm64=ed381ab433bfe580129eb680eff2332233dbb6c56c078cc5186a1cae49a1eb11
ARG TARGETARCH

RUN set -eu; \
    case "${TARGETARCH}" in \
      amd64) sha="${SPIFFEX_SHA256_amd64}" ;; \
      arm64) sha="${SPIFFEX_SHA256_arm64}" ;; \
      *) echo "unsupported architecture ${TARGETARCH}" >&2; exit 1 ;; \
    esac; \
    archive="spiffex_${SPIFFEX_VERSION}_linux_${TARGETARCH}.tar.gz"; \
    curl -fsSL -o "/tmp/${archive}" \
      "https://github.com/PeterSR/spiffex/releases/download/v${SPIFFEX_VERSION}/${archive}"; \
    echo "${sha}  /tmp/${archive}" | sha256sum -c -; \
    tar -xzf "/tmp/${archive}" -C /usr/local/bin spiffex; \
    mkdir -p /usr/share/licenses/spiffex; \
    tar -xzf "/tmp/${archive}" -C /usr/share/licenses/spiffex LICENSE; \
    rm "/tmp/${archive}"; \
    spiffex version

COPY rootfs /

LABEL \
  io.hass.type="addon" \
  io.hass.arch="aarch64|amd64" \
  org.opencontainers.image.title="spiffex agent" \
  org.opencontainers.image.description="A SPIFFE identity for Home Assistant, from a spiffex issuer" \
  org.opencontainers.image.source="https://github.com/PeterSR/homeassistant-spiffex-addon" \
  org.opencontainers.image.licenses="MIT AND Apache-2.0"
