# Builder
FROM arm32v7/golang as builder

ENV MIKROTIK_EXPORTER_VERSION "master"

RUN apt-get update && apt-get install -y git dmsetup && apt-get clean

RUN git clone --branch ${MIKROTIK_EXPORTER_VERSION} https://github.com/nshttpd/mikrotik-exporter.git /go/src/github.com/nshttpd/mikrotik-exporter

WORKDIR /go/src/github.com/nshttpd/mikrotik-exporter

RUN make build

# Image for usage
FROM arm32v7/debian

COPY --from=builder /go/src/github.com/nshttpd/mikrotik-exporter/mikrotik-exporter /app/mikrotik-exporter

RUN chown 1000:1000 -R /app

RUN chmod +x /app/mikrotik-exporter

EXPOSE 9436

ENTRYPOINT ["/app/mikrotik-exporter", "-config-file", "/app/conf/config.yml"]
