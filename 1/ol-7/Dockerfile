FROM bitnami/oraclelinux-extras:7-r13 as buildenv

RUN install_packages ca-certificates glibc wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/cluster-autoscaler-1.3.0-0-linux-x86_64-ol-7.tar.gz && \
    echo "577f757a29d61daf9f4d658c5bb4f7b20890564321f88d22c63126b62c4788e5  /tmp/bitnami/pkg/cache/cluster-autoscaler-1.3.0-0-linux-x86_64-ol-7.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/cluster-autoscaler-1.3.0-0-linux-x86_64-ol-7.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/cluster-autoscaler-1.3.0-0-linux-x86_64-ol-7.tar.gz

##################

FROM bitnami/oraclelinux-extras:7-r13
LABEL maintainer "Bitnami <containers@bitnami.com>"

COPY --from=buildenv /opt/bitnami/cluster-autoscaler /opt/bitnami/cluster-autoscaler
ENV BITNAMI_APP_NAME="cluster-autoscaler" \
    BITNAMI_IMAGE_VERSION="1.3.0-ol-7-r1" \
    PATH="/opt/bitnami/cluster-autoscaler/bin:$PATH"

RUN install_packages ca-certificates
RUN chmod -R g+rwX /opt/bitnami/cluster-autoscaler/

COPY rootfs /
WORKDIR /opt/bitnami/cluster-autoscaler
USER 1001
CMD ["/run.sh"]