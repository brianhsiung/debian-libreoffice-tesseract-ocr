FROM openjdk:8u332-jre-slim

LABEL maintainer=brianhsiung@outlook.com

ENV LIBREOFFICE_VERSION=7.2.7 \
    LIBREOFFICE_HOME=/opt/libreoffice7.2 \
    TESSDATA_PREFIX=/usr/share/tesseract-ocr/4.00/tessdata 

# base settings
ENV DEBIAN_FRONTEND noninteractive
RUN set -eux; \
    sed -i -e 's/deb.debian.org/mirrors.163.com/g' \
        -e 's/security.debian.org/mirrors.163.com/g' /etc/apt/sources.list; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
        lsb-release \
        apt-transport-https \
        ca-certificates \
        curl \
        procps \
        fontconfig \
        locales \
        wget \
        gnupg2 \
    ; \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen; \
    dpkg-reconfigure --frontend=noninteractive locales; \
    update-locale LANG=en_US.UTF-8; \
    rm -rf /etc/localtime; ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime; \
    rm -rf /var/lib/apt/lists/*

# Libreoffice7.2 
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libdbus-1-3 \
        libglib2.0-0 \
        libxinerama1 \
        libcups2 \
        libcairo2 \
        libsm6 \
        libx11-xcb1 \
    ; \ 
    wget -P /tmp "https://mirrors.cloud.tencent.com/libreoffice/libreoffice/stable/7.2.7/deb/x86_64/LibreOffice_7.2.7_Linux_x86-64_deb.tar.gz"; \
    wget -P /tmp "https://mirrors.cloud.tencent.com/libreoffice/libreoffice/stable/7.2.7/deb/x86_64/LibreOffice_7.2.7_Linux_x86-64_deb_langpack_zh-CN.tar.gz"; \ 
    tar Czxf /tmp /tmp/LibreOffice_*_Linux_x86-64_deb.tar.gz; \
    dpkg -i /tmp/LibreOffice_*_Linux_x86-64_deb/DEBS/*.deb; \
    tar Czxf /tmp /tmp/LibreOffice_*_Linux_x86-64_deb_langpack_zh-CN.tar.gz; \
    dpkg -i /tmp/LibreOffice_*_Linux_x86-64_deb_langpack_zh-CN/DEBS/*.deb; \
    rm -rf /var/lib/apt/lists/*
    
ENV LANG=en_US.UTF-8 \
    PATH=$PATH:$LIBREOFFICE_HOME/program

# tesseract-ocr4
RUN set -eux; \
    echo "deb https://notesalexp.org/tesseract-ocr4/$(lsb_release -cs)/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/notesalexp.list; \
    wget -O /tmp/keyfile https://notesalexp.org/debian/alexp_key.asc; \
    apt-key add /tmp/keyfile; \
    apt-get update; \
    apt-get install -y tesseract-ocr; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    apt-get remove -y wget gnupg2 lsb-release; \
    apt-get autoremove -y; \
    apt-get clean; \
    rm -rf /var/cache/apt/*; \
    rm -rf /tmp/*