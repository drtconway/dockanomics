FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y tzdata && \
    apt install -y \
        python3-distutils \
        multiqc 

RUN mkdir /data
WORKDIR /data
