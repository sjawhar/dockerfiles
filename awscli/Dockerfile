ARG AWSCLI_VERSION=2.13.20
FROM amazon/aws-cli:${AWSCLI_VERSION} as awscli

FROM debian:bullseye-20221114
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        ca-certificates \
        groff \
        less \
        wget \
 && rm -rf /var/lib/apt/lists/*

COPY --from=awscli /usr/local/aws-cli/v2/current /usr/local

RUN addgroup --gid 1000 aws \
 && adduser \
    --disabled-password \
    --gecos nte \
    --gid 1000 \
    --uid 1000 \
    aws

USER aws
ENTRYPOINT ["aws"]
CMD ["help"]
