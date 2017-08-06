FROM golang:1.8 as builder


ENV VERSION=0.8.0

RUN mkdir -p /go/src

RUN set -x \
    && cd /go/src \
    && git clone https://github.com/go-graphite/carbonapi.git

# build carbonapi
WORKDIR /go/src/carbonapi
RUN git checkout ${VERSION}
RUN go-wrapper download


RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o carbonapi .



FROM alpine

COPY --from=builder /go/src/carbonapi/carbonapi /sbin/

COPY entrypoint.sh /entrypoint.sh
COPY carbonapi.yaml /etc/carbonapi.yaml

EXPOSE 8080
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "/sbin/carbonapi", "-config", "/etc/carbonapi.yaml"]
