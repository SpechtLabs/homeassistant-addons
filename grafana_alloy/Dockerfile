ARG BUILD_FROM
FROM $BUILD_FROM

ARG ALLOY_VERSION
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

RUN apk --update add libc6-compat && rm -rf /var/cache/apk/*

RUN cd /tmp && \
    curl -sSLf -o alloy.zip "https://github.com/grafana/alloy/releases/download/${ALLOY_VERSION}/alloy-linux-${BUILD_ARCH//aarch64/arm64}.zip" && \
    unzip alloy.zip && mv alloy-linux-* /usr/bin/grafana-alloy && \
    rm alloy.zip

# Copy files for addon
COPY rootfs /
