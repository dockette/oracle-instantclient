# Oracle InstantClient

Please follow up rules on the Oracle website (x86 http://www.oracle.com/technetwork/topics/linuxsoft-082809.html) (x64 http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html).

## Goal

This repository provides easy to download Oracle instantclient used in Dockerfiles.

Thank you.

## Installation

Take a look at [installation guide for Ubuntu](https://help.ubuntu.com/community/Oracle%20Instant%20Client).

## Usage

### Legacy example (PHP 7.2)

Here is example Dockerfile, where you can find PHP 7.2 with OCI8 extension and Oracle instantclient v12.2.0.1.0.

```Dockerfile
FROM dockette/debian:stretch

ENV LD_LIBRARY_PATH="/usr/local/lib;/usr/local/instantclient"
ENV NLS_LANG="CZECH_CZECH REPUBLIC.AL32UTF8"

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
        ssh \
        wget \
        make \
        autoconf \
        g++ \
        unzip \
        libaio1 \
        ca-certificates \
        php7.2-dev && \
   rm /etc/nginx/sites.d/default

RUN wget https://github.com/dockette/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.2.0.1.0.zip -O /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip && \
    wget https://github.com/dockette/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.2.0.1.0.zip -O /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip && \
    unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/ && \
    unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/

RUN ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
    ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so && \
    echo 'instantclient,/usr/local/instantclient' | pecl install oci8 && \
    echo "extension=oci8.so" > /etc/php/7.2/cli/conf.d/20-oci8.ini && \
    echo "extension=oci8.so" > /etc/php/7.2/fpm/conf.d/20-oci8.ini

RUN apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get remove -y wget make autoconf g++ unzip && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/*
```
### Example for PHP 8.5

```Dockerfile
FROM debian:bookworm-slim

ENV LD_LIBRARY_PATH="/usr/local/lib:/usr/local/instantclient"
ENV NLS_LANG="AMERICAN_AMERICA.AL32UTF8"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        wget \
        unzip \
        libaio1 \
        php8.5-cli \
        php8.5-dev \
        php-pear \
        make \
        gcc \
        libc-dev && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/dockette/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.2.0.1.0.zip -O /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip && \
    wget https://github.com/dockette/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.2.0.1.0.zip -O /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip && \
    unzip /tmp/instantclient-basic-linux.x64-12.2.0.1.0.zip -d /usr/local/ && \
    unzip /tmp/instantclient-sdk-linux.x64-12.2.0.1.0.zip -d /usr/local/

RUN ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
    ln -s /usr/local/instantclient/libclntsh.so.12.1 /usr/local/instantclient/libclntsh.so && \
    printf 'instantclient,/usr/local/instantclient\n' | pecl install oci8 && \
    echo "extension=oci8.so" > /etc/php/8.5/cli/conf.d/20-oci8.ini

RUN apt-get purge -y wget unzip make gcc libc-dev php8.5-dev && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /tmp/* /var/tmp/*
```

## Maintenance

See [how to contribute](https://github.com/dockette/.github/blob/master/CONTRIBUTING.md) to this package. Consider to [support](https://github.com/sponsors/f3l1x) **f3l1x**. Thank you for using this package.
