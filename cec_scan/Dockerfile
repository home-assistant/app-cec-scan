# Base image updated by Renovate, update versionCompatibility on Alpine base bump
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.23-2026.10.1@sha256:9fd5d40f0682f0dee626cd0d2ea07b96ffef345dac1c6f025d4b4ce30ad362b4
FROM $BUILD_FROM

# Build libcec for HDMI-CEC
ARG LIBCEC_VERSION=7.1.1
WORKDIR /usr/src
RUN \
    apk add --no-cache \
        eudev-libs\
        p8-platform \
    && apk add --no-cache --virtual .build-dependencies \
        build-base \
        cmake \
        eudev-dev \
        git \
        p8-platform-dev \
        swig \
        linux-headers \
    && git clone --depth 1 -b libcec-${LIBCEC_VERSION} \
        "https://github.com/Pulse-Eight/libcec" libcec \
    && mkdir -p libcec/build \
    && cd libcec/build \
    && cmake \
        -DCMAKE_INSTALL_PREFIX:PATH=/usr/local \
        -DHAVE_LINUX_API=1 \
        .. \
    && make -j$(nproc) \
    && make install \
    && apk del --no-cache .build-dependencies \
    && rm -rf /usr/src/*

# Copy data
WORKDIR /
COPY rootfs /

LABEL \
    org.opencontainers.image.title="Home Assistant App: CEC Scanner" \
    org.opencontainers.image.description="Scan for HDMI CEC devices" \
    org.opencontainers.image.source="https://github.com/home-assistant/app-cec-scan" \
    org.opencontainers.image.licenses="Apache License 2.0"
