FROM ubuntu:18.04

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    build-essential \
    python3.8-dev \
    python3-pip \
    curl \
    gcc

ENV XAPIAN_VERSION=1.4.22
ENV PATH="/venv/bin:$PATH"

WORKDIR /tmp/

RUN pip3 install sphinx \
    ## Compile & Install Xapian
    && mkdir -p /tmp/src\
    # build xapian-core
    && curl -SL https://oligarchy.co.uk/xapian/${XAPIAN_VERSION}/xapian-core-${XAPIAN_VERSION}.tar.xz \
    | tar -xJC /tmp/src \
    && /tmp/src/xapian-core-${XAPIAN_VERSION}/configure --prefix=/venv \
    && make \
    && make install