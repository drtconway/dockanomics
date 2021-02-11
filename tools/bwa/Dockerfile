FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y \
        bwa 

RUN mkdir /data
WORKDIR /data
