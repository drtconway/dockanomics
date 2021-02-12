FROM ubuntu:groovy

ARG BPIPE_VERSION=0.9.10

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y \
        build-essential \
        cryptsetup-bin \
        golang-go \
        libseccomp-dev \
        openjdk-8-jdk-headless \
        pkg-config \
        squashfs-tools \
        tzdata \
        wget

# Install bpipe from github.
#
RUN cd /opt && \
    wget -q https://github.com/ssadedin/bpipe/releases/download/0.9.10/bpipe-${BPIPE_VERSION}.tar.gz && \
    tar zxf bpipe-${BPIPE_VERSION}.tar.gz && \
    ln -s bpipe-${BPIPE_VERSION} bpipe
ENV PATH=/opt/bpipe/bin:${PATH}

# Install singularity
#
ENV SINGULARITY_VERSION=3.7.1
ENV GOPATH=/root/go
RUN mkdir -p "${GOPATH}/src/github.com/sylabs" && \
    cd "${GOPATH}/src/github.com/sylabs" && \
    wget -q https://github.com/hpcng/singularity/releases/download/v3.7.1/singularity-${SINGULARITY_VERSION}.tar.gz && \
    tar -xzvf singularity-${SINGULARITY_VERSION}.tar.gz && \
    rm singularity-${SINGULARITY_VERSION}.tar.gz && \
    cd singularity && \
    ./mconfig -v -p /usr/local && \
    make -C builddir && \
    make -C builddir install && \
    rm -rf builddir

# Add a simple script for pulling images
#
ADD siffy /usr/local/bin/

RUN mkdir /data
WORKDIR /data
