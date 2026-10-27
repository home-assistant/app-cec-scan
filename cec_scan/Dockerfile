# Base image updated by Renovate, update versionCompatibility on Alpine base bump
ARG BUILD_FROM=ghcr.io/home-assistant/base:3.23-2026.10.3@sha256:1550b137f45bbb61f932a5ec97265c1d8594ddc4a986b127fb06a5c8c4bcaa6d
FROM $BUILD_FROM

# Build libcec for HDMI-CEC
ARG LIBCEC_VERSION=8.1.7
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
