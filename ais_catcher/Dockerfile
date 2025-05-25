# -------------------
# The build container
# -------------------
FROM debian:bullseye-slim AS build

RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get install git make gcc g++ cmake pkg-config -y
RUN apt-get install librtlsdr-dev libairspy-dev libhackrf-dev libairspyhf-dev libzmq3-dev libsoxr-dev libcurl4-openssl-dev zlib1g-dev libpq-dev -y

RUN git clone https://github.com/jvde-github/AIS-catcher.git /root/AIS-catcher

# COPY . /root/AIS-catcher

RUN cd /root/AIS-catcher; mkdir build; cd build; cmake ..; make; make install

# -------------------------
# The application container
# -------------------------
ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.8.3
# FROM ${BUILD_FROM}

FROM ghcr.io/hassio-addons/debian-base:7.8.3
# hadolint ignore=DL3006


RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get install librtlsdr0 libairspy0 libhackrf0 libairspyhf1 libzmq5 libsoxr0 libcurl4 libpq-dev -y

COPY --from=build /usr/local/bin/AIS-catcher /usr/local/bin/AIS-catcher
COPY ./run.sh /
RUN chmod +x /run.sh 

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Damon Vincent <damon@ourcomputershop.com>"

CMD ["./run.sh"]