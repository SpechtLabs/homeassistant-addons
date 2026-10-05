# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
# Debian rather than Alpine for libsystemd, which loki.source.journal needs to
# read the host's journal. The base image is a multi-platform one: BuildKit
# picks the variant for the platform it builds.
ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:bookworm
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# renovate: datasource=github-releases depName=grafana/alloy
ARG ALLOY_VERSION=v1.20.1
# Set by BuildKit: amd64 or arm64, which is how Alloy names its archives.
ARG TARGETARCH

# Distribution packages follow the base image, which Renovate keeps current;
# pinning their versions would break the build on every base image update.
# hadolint ignore=DL3008
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libsystemd0 \
        ca-certificates \
        curl \
        unzip \
    && rm -rf /var/lib/apt/lists/*

# The release zip ships the binary without the executable bit.
WORKDIR /tmp
RUN curl -sSLf -o alloy.zip \
        "https://github.com/grafana/alloy/releases/download/${ALLOY_VERSION}/alloy-linux-${TARGETARCH}.zip" \
    && unzip alloy.zip \
    && mv alloy-linux-* /usr/bin/grafana-alloy \
    && chmod +x /usr/bin/grafana-alloy \
    && rm alloy.zip
WORKDIR /

# Copy files for addon
COPY rootfs /

# io.hass.name, io.hass.description, io.hass.arch and io.hass.version come
# from config.yaml at build time; see .github/workflows/build-addon.yaml.
LABEL \
    io.hass.type="app" \
    maintainer="Cedi <cedi@specht-labs.de>" \
    org.opencontainers.image.title="Home Assistant Add-on: Grafana Alloy" \
    org.opencontainers.image.description="Grafana Alloy Addon." \
    org.opencontainers.image.licenses="Apache License 2.0" \
    org.opencontainers.image.vendor="Specht Labs"
