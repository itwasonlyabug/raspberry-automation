FROM rust:1.48 AS librespot-make

ARG LIBRESPOT_VERSION="0.2.0"
ARG ARCH="armhf"

RUN apt update \
 && apt -y install build-essential libasound2-dev curl unzip \
 && apt clean && rm -fR /var/lib/apt/lists

RUN cargo install librespot --version "${LIBRESPOT_VERSION}"

FROM ubuntu:latest

ARG SNAPCAST_VERSION="0.25.0"
ARG ARCH="armhf"

RUN apt update -y && apt install curl ca-certificates libasound2 avahi-utils libflac8 libogg0 libopus0 libsoxr0 libvorbis0a libvorbisenc2 -y
#RUN echo exit 0 > /usr/sbin/policy-rc.d
RUN curl -L -o /tmp/snapserver.deb "https://github.com/badaix/snapcast/releases/download/v${SNAPCAST_VERSION}/snapserver_${SNAPCAST_VERSION}-1_${ARCH}.deb" \
     && dpkg -i /tmp/snapserver.deb || apt install -f -y --no-install-recommends \
     && apt clean && rm -fR /var/lib/apt/lists

COPY snapserver.conf /etc/snapserver.conf
COPY --from=librespot-make /usr/local/cargo/bin/librespot /usr/local/bin/

CMD ["snapserver"]
EXPOSE 1704/tcp 1705/tcp 1780
