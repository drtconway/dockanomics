FROM ubuntu:groovy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y \
        git pandoc r-base texlive-luatex texlive-latex-extra \
        build-essential \
        libcurl4-gnutls-dev \
        libxml2-dev \
        libssl-dev \
        zlib1g-dev
RUN R -e 'update.packages(ask=FALSE)'
RUN R -e 'install.packages("devtools")' && \
    R -e 'install.packages("BiocManager")' && \
    R -e 'install.packages(c("data.table", "ggplot2", "gridExtra", "hexbin"))' && \
    R -e 'install.packages(c("rmarkdown", "knitr", "kableExtra"))' && \
    R -e 'BiocManager::install("ComplexHeatmap")' && \
    R -e 'BiocManager::install("DESeq2")' && \
    R -e 'BiocManager::install("PCAtools")'

RUN mkdir /data
WORKDIR /data
