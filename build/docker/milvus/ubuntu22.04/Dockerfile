FROM ubuntu:24.04 

ARG TARGETARCH=amd64

RUN apt-get update && \
apt-get install -y --no-install-recommends curl ca-certificates libaio-dev libgomp1 libopenblas-dev && \
apt-get remove --purge -y && \
rm -rf /var/lib/apt/lists/*

COPY ./bin/ /milvus/bin/

COPY ./configs/ /milvus/configs/

COPY ./internal/core/output/lib/ /milvus/lib/

ENV PATH=/milvus/bin:$PATH
ENV LD_LIBRARY_PATH=/milvus/lib:$LD_LIBRARY_PATH:/usr/lib
ENV LD_PRELOAD=/milvus/lib/libjemalloc.so
ENV MALLOC_CONF=background_thread:true

ADD https://github.com/krallin/tini/releases/download/v0.19.0/tini-$TARGETARCH /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]

WORKDIR /milvus/

CMD ["milvus", "run", "standalone"]