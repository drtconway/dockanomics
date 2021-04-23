FROM ubuntu:groovy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y \
        curl \
        r-base \
        libssl-dev \
        libcurl4-openssl-dev \
        libxml2-dev
RUN R -e 'update.packages(ask=FALSE)'
RUN R -e 'install.packages("data.table")'
RUN R -e 'install.packages("doBy")'
RUN R -e 'install.packages("docopt")'
RUN R -e 'install.packages("ggplot2")'
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'BiocManager::install("DESeq2")'

RUN mkdir /data
WORKDIR /data
