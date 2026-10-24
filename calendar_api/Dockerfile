# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
# The base image is a multi-platform one: BuildKit picks the variant for the
# platform it builds.
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.24
FROM ${BUILD_FROM}

SHELL ["/bin/ash", "-o", "pipefail", "-c"]

# renovate: datasource=github-releases depName=SpechtLabs/CalendarAPI
ARG CALENDAR_API_VERSION=v0.1.9
# Set by BuildKit: amd64 or arm64, which is how CalendarAPI names its archives.
ARG TARGETARCH

# Distribution packages follow the base image, which Renovate keeps current;
# pinning their versions would break the build on every base image update.
# hadolint ignore=DL3018
RUN apk add --no-cache libc6-compat

WORKDIR /tmp
RUN curl -sSLf -o calendarapi.tar.gz \
        "https://github.com/SpechtLabs/CalendarAPI/releases/download/${CALENDAR_API_VERSION}/CalendarAPI_${CALENDAR_API_VERSION#v}_linux_${TARGETARCH}.tar.gz" \
    && tar -xzf calendarapi.tar.gz calendarapi \
    && mv calendarapi /usr/bin/calendarapi \
    && rm calendarapi.tar.gz
WORKDIR /

# Copy files for addon
COPY rootfs /

EXPOSE 8099
EXPOSE 50051

# io.hass.name, io.hass.description, io.hass.arch and io.hass.version come
# from config.yaml at build time; see .github/workflows/build-addon.yaml.
LABEL \
    io.hass.type="addon" \
    maintainer="Cedi <cedi@specht-labs.de>" \
    org.opencontainers.image.title="Home Assistant Add-on: Meeting E-Paper Display (EPD) Server" \
    org.opencontainers.image.description="A lightweight, configurable server to serve iCal calendars over gRPC and REST for the E-Paper meeting-room display project." \
    org.opencontainers.image.licenses="BSD 3-Clause" \
    org.opencontainers.image.vendor="Specht Labs"
