FROM ubuntu:groovy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y \
        r-base \
        build-essential \
        libcurl4-gnutls-dev \
        libxml2-dev \
        libssl-dev \
        zlib1g-dev \
        libmariadbd-dev
RUN R -e 'update.packages(ask=FALSE)'
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'BiocManager::install("FRASER")'

RUN mkdir /data
WORKDIR /data
