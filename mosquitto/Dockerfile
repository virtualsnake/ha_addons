ARG BUILD_FROM
FROM $BUILD_FROM

# Install mosquitto + auth plugin
WORKDIR /usr/src
ARG MOSQUITTO_AUTH_VERSION
RUN apt-get update \
    && apt-get install -qy --no-install-recommends \
        mosquitto \
        nginx \
        pwgen \
        build-essential \
        git \
        mosquitto-dev \
        libmosquitto-dev \
        openssl \
        libssl-dev \
        golang-go \
        docbook-xsl \
        libc-ares-dev \
        libcjson-dev \
        libcjson1 \
        libcunit1-dev \
        libssl-dev \
        libwrap0-dev \
        microsocks \
        python-all \
        python3-all \
        xsltproc git make gcc g++ libsqlite3-dev \
    \
    && git clone --depth 1 -b "${MOSQUITTO_AUTH_VERSION}" \
        https://github.com/iegomez/mosquitto-go-auth \
    \
    && cd mosquitto-go-auth \
    && sed -i 's/-I\/usr\/local\/include/-I\/usr\/include/' Makefile \
    && sed -i 's/LDFLAGS := .*$/& -Wl,-unresolved-symbols=ignore-all/' Makefile \
    && make \
    && mkdir -p /usr/share/mosquitto \
    && cp -f go-auth.so /usr/share/mosquitto \
    && cp -f pw /usr/local/bin \
    && cd .. \
    \
    && git clone https://github.com/eclipse/mosquitto.git \
    && cd mosquitto \
    && git checkout develop \
    && make install \
    && cd .. \
    \
    && apt-get purge -y --auto-remove \
        build-essential \
        git \
        mosquitto-dev \
        libmosquitto-dev \
        libssl-dev \
        golang-go \
        docbook-xsl \
        libc-ares-dev \
        libcjson-dev \
        libcjson1 \
        libcunit1-dev \
        libssl-dev \
        libwrap0-dev \
        microsocks \
        python-all \
        python3-all \
        xsltproc git make gcc g++ libsqlite3-dev \
    && apt-get clean \
    && rm -fr \
        /etc/logrotate.d \
        /etc/mosquitto/* \
        /etc/nginx/* \
        /usr/share/nginx \
        /usr/src/mosquitto-go-auth \
        /var/lib/nginx/html \
        /var/www \
        /var/lib/apt/lists/* \
        /root/.cache \
        /root/go

# Copy rootfs
COPY rootfs /

WORKDIR /
