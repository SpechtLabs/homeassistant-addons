ARG BUILD_FROM

FROM $BUILD_FROM

ARG CALENDAR_API_VERSION
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
    STRIPPED_VERSION=${CALENDAR_API_VERSION#v} &&  \
    curl -sSLf -o calendarapi.tar.gz "https://github.com/SpechtLabs/CalendarAPI/releases/download/${CALENDAR_API_VERSION}/CalendarAPI_${STRIPPED_VERSION}_linux_${BUILD_ARCH//aarch64/arm64}.tar.gz" && \
    tar -xvzf calendarapi.tar.gz && mv calendarapi /usr/bin/calendarapi && \
    rm calendarapi.tar.gz

# Copy files for addon
COPY rootfs /

EXPOSE     8099
EXPOSE     50051