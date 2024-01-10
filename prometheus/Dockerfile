ARG BUILD_FROM=ghcr.io/hassio-addons/base:15.0.4
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64
ENV PROMETHEUS_VERSION=2.48.1

# Copy root filesystem
COPY rootfs /

# hadolint ignore=DL3003
RUN \
    ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    \
    && curl -J -L -o /tmp/prometheus.tar.gz \
    "https://github.com/prometheus/prometheus/releases/download/v${PROMETHEUS_VERSION}/prometheus-${PROMETHEUS_VERSION}.linux-${ARCH}.tar.gz" \
    && cd /tmp \
    && tar -xvf /tmp/prometheus.tar.gz \
    && mkdir -p /etc/prometheus \
    && cp prometheus-${PROMETHEUS_VERSION}.linux-${ARCH}/promtool /usr/local/bin/ \
    && cp prometheus-${PROMETHEUS_VERSION}.linux-${ARCH}/prometheus /usr/local/bin/ \
    && cp -R prometheus-${PROMETHEUS_VERSION}.linux-${ARCH}/console_libraries/ /etc/prometheus/ \
    && cp -R prometheus-${PROMETHEUS_VERSION}.linux-${ARCH}/consoles/ /etc/prometheus/ \
    && rm -r prometheus-${PROMETHEUS_VERSION}.linux-${ARCH} \
    && rm -f -r \
    /tmp/*

# Build arguments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Prometheus" \
    io.hass.description="Monitoring system & time series database" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="anrgistan <git@slange.me>" \
    org.opencontainers.image.title="Prometheus" \
    org.opencontainers.image.description="Monitoring system & time series database" \
    org.opencontainers.image.vendor="github.com/angristan" \
    org.opencontainers.image.authors="anrgistan <git@slange.me>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/angristan/fork-addon-prometheus" \
    org.opencontainers.image.source="https://github.com/angristan/fork-addon-prometheus" \
    org.opencontainers.image.documentation="https://github.com/angristan/fork-addon-prometheus/blob/master/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
