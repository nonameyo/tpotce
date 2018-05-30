FROM alpine

# Setup apk
RUN apk -U --no-cache add \
               build-base \
               git \
               go && \

# Setup go, vnclowpot
    export GOPATH=/opt/go/ && \
    mkdir -p /opt/go/ && \
    cd /opt && \
    go get github.com/magisterquis/vnclowpot && \
    go install github.com/magisterquis/vnclowpot && \

# Setup user, groups and configs
    mkdir -p /var/log/vnclowpot && \

# Clean up
    apk del build-base \
            git \
            go && \
    rm -rf /opt/go/src \
           /var/cache/apk/* \

# Run vnclowpot
EXPOSE 5900
USER nobody:nobody
CMD exec /opt/go/bin/vnclowpot -j >> /tmp/sensor/vnclowpot.log
