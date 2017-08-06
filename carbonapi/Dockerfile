FROM golang:1.8-alpine


ENV VERSION=0.8.0

RUN mkdir -p /go/src

ADD https://github.com/go-graphite/carbonapi/archive/${VERSION}.zip /tmp/carbonapi.zip

# build carbonapi
RUN set -x                                           \
    && apk add --update git                          \
    && cd /go/src                                    \
    && unzip /tmp/carbonapi.zip                      \
    && mv /go/src/carbonapi-* /go/src/carbonapi      \
    && cd /go/src/carbonapi                          \
    && go-wrapper download                           \
    && go-wrapper install                            \
    && apk del git                                   \
    && rm -f /tmp/carbonapi.zip                      \
    && rm -rf /var/cache/apk/*

EXPOSE 8080

COPY entrypoint.sh /entrypoint.sh
COPY carbonapi.yaml /etc/carbonapi.yaml

ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "/go/bin/carbonapi", "-config", "/etc/carbonapi.yaml"]
