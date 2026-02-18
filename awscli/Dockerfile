ARG AWSCLI_VERSION=2.33.24
FROM amazon/aws-cli:${AWSCLI_VERSION} AS awscli

FROM debian:trixie-20260202-slim
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    rm -f /etc/apt/apt.conf.d/docker-clean \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
        ca-certificates \
        groff \
        less \
        wget

COPY --from=awscli /usr/local/aws-cli/v2/current /usr/local

ARG TARGETARCH
RUN ARCH=$([ "$TARGETARCH" = "arm64" ] && echo "arm64" || echo "64bit") \
 && wget -q -O /tmp/session-manager-plugin.deb \
        "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_${ARCH}/session-manager-plugin.deb" \
 && dpkg -i /tmp/session-manager-plugin.deb \
 && rm /tmp/session-manager-plugin.deb \
 && ln -sf /usr/local/sessionmanagerplugin/bin/session-manager-plugin /usr/local/bin/session-manager-plugin

RUN groupadd --gid 1000 aws \
 && useradd \
    --no-log-init \
    --comment nte \
    --gid 1000 \
    --uid 1000 \
    --create-home \
    aws

USER aws
ENTRYPOINT ["aws"]
CMD ["help"]
