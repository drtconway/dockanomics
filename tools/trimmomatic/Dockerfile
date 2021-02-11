FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y tzdata && \
    apt install -y \
        trimmomatic 

RUN mkdir /data
WORKDIR /data
