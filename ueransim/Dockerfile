FROM debian:latest as builder

ARG version=v3.2.2

ENV VERSION=$version

RUN apt-get update && \
    apt-get install -y  \
    git \
    make \
    wget \
    g++ \
    libssl-dev \
    libsctp-dev 
    
# requires cmake >=3.17
RUN wget https://github.com/Kitware/CMake/releases/download/v3.17.3/cmake-3.17.3.tar.gz && \
    tar -zxvf cmake-3.17.3.tar.gz && \ 
    cd cmake-3.17.3 && \
    ./bootstrap && \
    make && \
    make install

RUN file="$(cmake --version)" && echo $file


RUN cd /tmp && git clone https://github.com/aligungr/UERANSIM.git && \
    cd UERANSIM && git checkout tags/$VERSION 
RUN cd /tmp/UERANSIM && echo "cmake --version" && make

#FROM debian:stable-slim
FROM debian:buster-slim

RUN apt-get update && \
    apt-get install -y  \
    curl \
    dnsutils \
    gettext-base \
    iproute2 \
    iputils-ping \
    libsctp1 \
    traceroute tcpdump && \
    rm -rf /var/lib/apt/lists/*

    
COPY --from=builder /tmp/UERANSIM/build/* /usr/local/bin/


COPY /etc/ueransim /etc/ueransim
COPY entrypoint.sh /entrypoint.sh

ENV ENB_ID=0x19B \
    MCC=001 \
    MNC=01 \
    NETWORK=openverso.org \
    TAC=0001 \
    MSISDN=0000000001 \
    KEY=465B5CE8B199B49FAA5F0A2EE238A6BC \
    OP_TYPE=OPC \
    OP=E8ED289DEBA952E4283B54E88E6183CA \
    SST=1 \
    SD=1 \
    UE_HOSTNAME=ue \
    GNB_HOSTNAME=gnb \
    AMF_HOSTNAME=amf \
    APN=internet

ENTRYPOINT ["/entrypoint.sh"]
CMD ["/bin/sh"]


