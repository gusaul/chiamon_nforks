FROM golang:alpine AS builder

WORKDIR /build
RUN apk add --update --no-cache --virtual build-dependencies git make \
 && git clone https://github.com/gusaul/chia_exporter_nforks \
 && cd chia_exporter_nforks \
 && go build -tags netgo

FROM alpine
COPY --from=builder /build/chia_exporter_nforks/chia_exporter_nforks /usr/bin/chia_exporter_nforks
COPY --from=builder /build/chia_exporter_nforks/config.yaml /usr/bin/config.yaml
RUN sed -i "s|\$HOME|/etc/key|g" /usr/bin/config.yaml

ENTRYPOINT ["/usr/bin/chia_exporter_nforks", "-config", "/usr/bin/config.yaml"]
