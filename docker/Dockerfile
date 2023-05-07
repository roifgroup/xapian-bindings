ARG XAPIAN_VERSION
FROM papermerge/many-linux-with-xapian:${XAPIAN_VERSION}

ENV PREFIX=/venv
ENV XAPIAN_CONFIG=/venv/bin/xapian-config
ARG XAPIAN_VERSION
ENV PATH="/opt/python/cp310-cp310/bin:$PATH"
ENV PYBIN=/opt/python/cp310-cp310/bin

RUN ${PYBIN}/pip install sphinx

RUN mkdir -p tmp/src

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

# copy *.so AND __init__.py; at this point __init__.py  will contain SWIG interface
# __init__.py was generated in when building xapian bindings
RUN cp /opt/python/cp310-cp310/lib/python3.10/site-packages/xapian/*.so /app/xapian/
RUN cp /opt/python/cp310-cp310/lib/python3.10/site-packages/xapian/*.py /app/xapian/

RUN TODAY=$(date +"%y%m%d%H%m") && echo "__version__ = '${XAPIAN_VERSION}.post${TODAY}'" > /app/xapian/bindings.py

RUN python setup.py bdist_wheel
RUN auditwheel repair dist/*.whl
