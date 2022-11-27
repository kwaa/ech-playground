FROM debian:sid-slim as builder

WORKDIR /app

RUN set -ex; \
    apt update && apt install \
    build-essential \
    git; \
    # Download & Build OpenSSL (sftcd:ECH-draft-13a)
    git clone git@github.com:sftcd/openssl.git -b ECH-draft-13a --single-branch; \
    cd openssl; \
    ./config; \
    make; \
    # Download & Build HAProxy (sftcd:ECH-experimental)
    cd /app; \
    git clone git@github.com:sftcd/haproxy.git -b ECH-experimental --single-branch; \
    cd haproxy; \
    make -j $(nproc) \
    TARGET=linux-glibc \
    USE_OPENSSL=1 \
    SSL_INC=/app/openssl/include \
    SSL_LIB=/app/openssl \
    DEFINE="-DOPENSSL_SUPPRESS_DEPRECATED"

FROM debian:sid-slim

WORKDIR /usr/bin

RUN apt update && apt install ca-certificates

COPY --from=builder /app/haproxy/haproxy haproxy

CMD ["haproxy", "-f", "/etc/haproxy/haproxy.cfg"]