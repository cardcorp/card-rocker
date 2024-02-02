## Start with the official rocker image (lightweight Debian)
FROM cardcorp/r-aws:latest

MAINTAINER Gergely Daroczi <gergely.daroczi@card.com>

## dev packages required for curl etc
RUN apt-get update && apt-get install -y libssl-dev libssl-dev libcurl4-openssl-dev

## Install Java 
RUN mkdir -p /usr/share/binfmts \
  && apt update \
  && apt install -y openjdk-8-jdk \
  && R CMD javareconf

## make sure Java can be found in rApache and other daemons not looking in R ldpaths
RUN echo "/usr/lib/jvm/java-8-oracle/jre/lib/amd64/server/" > /etc/ld.so.conf.d/rJava.conf
RUN /sbin/ldconfig

## Install LaTeX
## Source: https://registry.hub.docker.com/u/rocker/hadleyverse/dockerfile/
RUN apt-get install -y --no-install-recommends \
      ghostscript \
      imagemagick \
      lmodern \
      texlive-fonts-recommended \
      texlive-humanities \
      texlive-latex-extra \
      texinfo \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/ \
  && cd /usr/share/texlive/texmf-dist \
  && wget http://mirrors.ctan.org/install/fonts/inconsolata.tds.zip \
  && unzip inconsolata.tds.zip \
  && rm inconsolata.tds.zip \
  && echo "Map zi4.map" >> /usr/share/texlive/texmf-dist/web2c/updmap.cfg \
  && mktexlsr \
  && updmap-sys

## Install Pandoc
RUN mkdir -p /opt/pandoc \
  && wget --no-check-certificate -O /tmp/pandoc.zip https://s3.amazonaws.com/rstudio-buildtools/pandoc-1.13.1.zip \
  && unzip -j /tmp/pandoc.zip "pandoc-1.13.1/linux/debian/x86_64/pandoc" -d /opt/pandoc \
  && chmod +x /opt/pandoc/pandoc \
  && ln -s /opt/pandoc/pandoc /usr/local/bin \
  && unzip -j /tmp/pandoc.zip "pandoc-1.13.1/linux/debian/x86_64/pandoc-citeproc" -d /opt/pandoc \
  && chmod +x "/opt/pandoc/pandoc-citeproc" \
  && ln -s "/opt/pandoc/pandoc-citeproc" /usr/local/bin \
  && rm -f /tmp/pandoc.zip

## Install related R packages
RUN install2.r --error \
    httr \ 
    rJava \
    AWR \
    knitr \
    rmarkdown \
    pander \
  && rm -rf /tmp/downloaded_packages/ /tmp/*.rds
