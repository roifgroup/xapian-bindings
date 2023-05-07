FROM quay.io/pypa/manylinux2014_x86_64

ENV PREFIX=/venv
ENV XAPIAN_CONFIG=/venv/bin/xapian-config
ENV XAPIAN_VERSION=1.4.22
ENV PATH="/opt/python/cp310-cp310/bin:$PATH"
ENV PYBIN=/opt/python/cp310-cp310/bin

RUN ${PYBIN}/pip install sphinx

RUN yum install -y curl

RUN mkdir -p tmp/src

# compile xapian core
RUN curl -SL https://oligarchy.co.uk/xapian/${XAPIAN_VERSION}/xapian-core-${XAPIAN_VERSION}.tar.xz \
    | tar -xJC /tmp/src \
    && /tmp/src/xapian-core-${XAPIAN_VERSION}/configure --prefix=${PREFIX}  \
    && make \
    && make install

# compile xapian python bindings
RUN curl -SL http://oligarchy.co.uk/xapian/${XAPIAN_VERSION}/xapian-bindings-${XAPIAN_VERSION}.tar.xz \
    | tar -xJC /tmp/src

RUN /tmp/src/xapian-bindings-${XAPIAN_VERSION}/configure \
    --prefix=${PREFIX} \
    --with-python3  \
    && make \
    && make install

COPY setup.py setup.cfg MANIFEST.in /app/
COPY xapian/ /app/xapian/

WORKDIR /app

RUN cp /opt/python/cp310-cp310/lib/python3.10/site-packages/xapian/*.so /app/xapian/
RUN python setup.py bdist_wheel
RUN auditwheel repair dist/*.whl
