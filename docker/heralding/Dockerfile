FROM alpine
  
# Include dist
ADD dist/ /root/dist/

# Install packages
RUN apk -U --no-cache add \ 
            		build-base \
            		git \
            		libcap \
            		libffi-dev \
            		libressl-dev \
                        libzmq \
            		postgresql-dev \
            		python3 \
            		python3-dev \
            		py-virtualenv && \
    pip3 install --no-cache-dir --upgrade pip && \

# Setup heralding
    mkdir -p /opt && \
    cd /opt/ && \
    git clone --depth=1 https://github.com/johnnykv/heralding && \
    cd heralding && \
    pip3 install --no-cache-dir -r requirements.txt && \
    pip3 install --no-cache-dir heralding && \

# Setup user, groups and configs
    mkdir -p /etc/heralding && \
    mv /root/dist/heralding.yml /etc/heralding/ && \
    setcap cap_net_bind_service=+ep /usr/bin/python3.6 && \

# Clean up
    apk del --purge \
            build-base \
            git \
            libcap \
            libffi-dev \
            libressl-dev \
            postgresql-dev \
            python3-dev \
            py-virtualenv && \
    rm -rf /root/* \
           /var/cache/apk/* \
           /opt/heralding

# Start elasticpot
WORKDIR /tmp/sensor/
EXPOSE 21 22 23 25 80 110 143 443 993 995 5432 5900
USER nobody:nobody
CMD exec heralding -c /etc/heralding/heralding.yml -l /tmp/sensor/heralding.log
