FROM ubuntu:groovy

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y tzdata && \
    apt install -y \
        openjdk-8-jdk-headless \
        wget && \
    wget -q https://github.com/ssadedin/bazam/releases/download/1.0.1/bazam.jar && \
    mkdir -p /usr/share/bazam && \
    mv bazam.jar /usr/share/bazam/
ADD bazam /usr/local/bin

RUN mkdir /data
WORKDIR /data
