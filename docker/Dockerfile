FROM ubuntu:18.04

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    build-essential \
    python3.8-dev \
    python3-pip \
    curl \
    gcc

ENV XAPIAN_VERSION=1.4.22

WORKDIR /tmp/

RUN pip3 install sphinx \
    && mkdir -p /tmp/src\
    && curl -SL https://oligarchy.co.uk/xapian/${XAPIAN_VERSION}/xapian-core-${XAPIAN_VERSION}.tar.xz \
    | tar -xJC /tmp/src \
    && /tmp/src/xapian-core-${XAPIAN_VERSION}/configure \
    && make \
    && make install