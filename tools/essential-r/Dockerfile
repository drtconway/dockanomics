FROM ubuntu:groovy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt install -y \
        git pandoc r-base texlive-luatex texlive-latex-extra \
        build-essential \
        libcurl4-gnutls-dev \
        libfontconfig1-dev \
        libssl-dev \
        libxml2-dev \
        zlib1g-dev
RUN R -e 'update.packages(ask=FALSE)'
RUN R -e 'install.packages("devtools")'
RUN R -e 'install.packages("BiocManager")'
RUN R -e 'install.packages(c("data.table", "ggplot2", "gridExtra", "hexbin"))'
RUN R -e 'install.packages(c("rmarkdown", "knitr", "kableExtra", "DT"))'
RUN R -e 'BiocManager::install("ComplexHeatmap")
RUN R -e 'BiocManager::install("DESeq2")'
RUN R -e 'BiocManager::install("FRASER")'
RUN R -e 'BiocManager::install("PCAtools")'

RUN mkdir /data
WORKDIR /data
