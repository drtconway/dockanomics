FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y tzdata && \
    apt install -y \
        build-essential \
        zlib1g-dev \
        wget

RUN wget -q https://github.com/alexdobin/STAR/archive/2.7.7a.tar.gz && \
    tar zxf 2.7.7a.tar.gz && \
    cd STAR-2.7.7a/source && \
    make && \
    cp STAR /usr/local/bin && \
    cd ../.. && \
    rm -rf STAR-2.7.7a/source

RUN mkdir /data
WORKDIR /data
