FROM debian:stretch-slim
ENV DEBIAN_FRONTEND noninteractive

# Include dist
ADD dist/ /root/dist/

# Install dependencies and packages
RUN apt-get update -y && \
    apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends \
      autoconf \
      automake \
      build-essential \
      ca-certificates \
      check \
      cython3 \
      git \
      libcap2-bin \
      libcurl4-openssl-dev \
      libemu-dev \
      libev-dev \
      libglib2.0-dev \
      libloudmouth1-dev \
      libnetfilter-queue-dev \
      libnl-3-dev \
      libpcap-dev \
      libssl-dev \
      libtool \
      libudns-dev \
      python3 \
      python3-dev \
      python3-bson \
      python3-yaml \
      ttf-liberation && \

# Get and install dionaea
    git clone --depth=1 https://github.com/dinotools/dionaea /root/dionaea/ && \
    cd /root/dionaea && \
    autoreconf -vi && \
    ./configure \
      --prefix=/opt/dionaea \
      --with-python=/usr/bin/python3 \
      --with-cython-dir=/usr/bin \
      --enable-ev \
      --with-ev-include=/usr/include \
      --with-ev-lib=/usr/lib \
      --with-emu-lib=/usr/lib/libemu \
      --with-emu-include=/usr/include \
      --with-nl-include=/usr/include/libnl3 \
      --with-nl-lib=/usr/lib \
      --enable-static && \
    make && \
    make install && \

# Supply configs and set permissions
    setcap cap_net_bind_service=+ep /opt/dionaea/bin/dionaea && \
    chown -R nobody:nogroup /opt/dionaea/var && \
    rm -rf /opt/dionaea/etc/dionaea/* && \
    mv /root/dist/etc/* /opt/dionaea/etc/dionaea/ && \

# Setup runtime and clean up
    apt-get purge -y \
      autoconf \
      automake \
      build-essential \
      ca-certificates \
      check \
      cython3 \
      git \
      libcurl4-openssl-dev \
      libemu-dev \
      libev-dev \
      libglib2.0-dev \
      libloudmouth1-dev \
      libnetfilter-queue-dev \
      libnl-3-dev \
      libpcap-dev \
      libssl-dev \
      libtool \
      libudns-dev \
      python3 \
      python3-dev \   
      python3-bson \
      python3-yaml && \

    apt-get install -y \
      ca-certificates \
      python3 \
      python3-bson \
      python3-yaml \
      libcurl3 \
      libemu2 \
      libev4 \
      libglib2.0-0 \
      libnetfilter-queue1 \
      libnl-3-200 \
      libpcap0.8 \
      libpython3.5 \
      libudns0 && \

    apt-get autoremove --purge -y && \
    apt-get clean && \
    rm -rf /root/* /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Start dionaea
EXPOSE 20 21 42 69/udp 80 135 443 445 1433 1723 1883 3306 5060 5060/udp 5061 27017
USER nobody:nogroup
CMD ["/opt/dionaea/bin/dionaea", "-u", "nobody", "-g", "nogroup", "-c", "/opt/dionaea/etc/dionaea/dionaea.cfg"]
