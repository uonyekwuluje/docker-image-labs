FROM ubuntu:22.04
MAINTAINER Uchechukwu Onyekwuluje <uionyekwuluje@gmail.com>

ARG BUILD_VERSION="v1.1.0"
ENV GOLANG_VERSION="1.22.0"

RUN apt update -y && apt upgrade -y 
RUN apt install -y wget jq curl dnsutils iputils-ping iproute2 openssl dumb-init percona-toolkit apt-utils bind9-doc \
    bind9utils whois cracklib-runtime redis-tools net-tools dnsutils build-essential ca-certificates lsb-release

RUN cd /tmp && \
    wget https://dl.google.com/go/go${GOLANG_VERSION}.linux-amd64.tar.gz && \
    tar -C /usr/local/ -xvf go${GOLANG_VERSION}.linux-amd64.tar.gz 

ENV PATH="/root/go/bin:/usr/local/go/bin:${PATH}"
LABEL build_version="$BUILD_VERSION" 
CMD ["/bin/bash"]
