FROM bitnami/minideb:bullseye as builder

COPY prebuildfs /

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install required system packages and dependencies
RUN install_packages ca-certificates gzip tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/oras-0.13.0-2-linux-amd64-debian-11.tar.gz && \
    echo "5559cb6df54d0b6b07bfb34ec4aa32b3cd1d807fef3b1a0552be1ef04906dea6  /tmp/bitnami/pkg/cache/oras-0.13.0-2-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/oras-0.13.0-2-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/oras-0.13.0-2-linux-amd64-debian-11.tar.gz

######

FROM scratch

COPY prebuildfs /
COPY rootfs /
COPY --from=builder /opt/bitnami/oras/bin/oras /oras

USER 1001

ENTRYPOINT [ "/oras" ]
