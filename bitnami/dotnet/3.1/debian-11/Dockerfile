FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/app" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.ref.name="3.1.28-debian-11-r2" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/dotnet" \
      org.opencontainers.image.title="dotnet" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="3.1.28"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages build-essential ca-certificates curl git gzip libbrotli1 libc6 libcap2-bin libcom-err2 libcurl4 libffi7 libgcc-s1 libgcrypt20 libgmp10 libgnutls30 libgpg-error0 libgssapi-krb5-2 libhogweed6 libicu-dev libidn2-0 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblttng-ust-dev libnettle8 libnghttp2-14 libp11-kit0 libpsl5 librtmp1 libsasl2-2 libsqlite3-dev libssh2-1 libssl-dev libssl1.1 libstdc++6 libtasn1-6 libunistring2 pkg-config procps tar unzip wget zlib1g
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/dotnet-3.1.28-0-linux-amd64-debian-11.tar.gz && \
    echo "806e8108a8bf2e18a82248921cb22fd84889bba137694fd36be47d5d2c523bee  /tmp/bitnami/pkg/cache/dotnet-3.1.28-0-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/dotnet-3.1.28-0-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/dotnet-3.1.28-0-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    0/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password

COPY rootfs /
RUN /opt/bitnami/scripts/dotnet/postunpack.sh
ENV APP_VERSION="3.1.28" \
    BITNAMI_APP_NAME="dotnet" \
    DOTNET_CLI_HOME="/app" \
    DOTNET_ROOT="/opt/bitnami/dotnet/bin" \
    PATH="/opt/bitnami/dotnet/bin:$PATH"

WORKDIR /app
ENTRYPOINT [ "/opt/bitnami/scripts/dotnet/entrypoint.sh" ]
CMD [ "/bin/bash" ]
