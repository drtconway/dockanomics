FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y \
        samtools \
        tabix

RUN mkdir /data
WORKDIR /data
