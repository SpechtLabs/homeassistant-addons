ARG CALENDAR_API_VERSION
FROM ghcr.io/spechtlabs/calendarapi:$CALENDAR_API_VERSION

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Cedi <cedi@specht-labs.de>"

ENTRYPOINT ["/bin/calendarapi", "serve"]

EXPOSE     8099
EXPOSE     50051