FROM alpine

# Include dist
ADD dist/ /root/dist/

# Setup env and apt
RUN apk -U --no-cache add\
            build-base \
            git \
            libffi \
            libffi-dev \
            openssl \
            openssl-dev \
            python3 \
            python3-dev && \

# Get and install packages
    mkdir -p /opt/ && \
    cd /opt/ && \
    git clone --depth=1 https://github.com/cymmetria/ciscoasa_honeypot && \
    cd ciscoasa_honeypot && \
    pip3 install --no-cache-dir --upgrade pip && \
    pip3 install --no-cache-dir -r requirements.txt && \
    cp /root/dist/asa_server.py /opt/ciscoasa_honeypot && \
    chown -R nobody:nobody /opt/ciscoasa_honeypot && \

# Clean up
    apk del --purge build-base \
                    git \
                    libffi-dev \
                    openssl-dev \
                    python3-dev && \
    rm -rf /root/* && \
    rm -rf /var/cache/apk/*

# Start ciscoasa
EXPOSE 5000/udp 8443
WORKDIR /tmp/sensor
USER nobody:nobody
CMD cp -R /opt/ciscoasa_honeypot/* /tmp/sensor && exec python3 asa_server.py --ike-port 5000 --enable_ssl --port 8443 --verbose
