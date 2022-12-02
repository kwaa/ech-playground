FROM alpine:edge as builder

WORKDIR /app

RUN set -ex; \
    apk add --no-cache --virtual deps \
    build-base \
    ca-certificates \
    git \
    linux-headers \
    perl; \
    # Download & Build OpenSSL (sftcd:ECH-draft-13a)
    git clone https://github.com/sftcd/openssl.git -b ECH-draft-13a --single-branch --depth=1; \
    cd openssl; \
    ./config; \
    make -j $(nproc); \
    # Download & Build HAProxy (sftcd:ECH-experimental)
    cd /app; \
    git clone https://github.com/sftcd/haproxy.git -b ECH-experimental --single-branch --depth=1; \
    cd haproxy; \
    make -j $(nproc) \
    TARGET=linux-musl \
    USE_OPENSSL=1 \
    SSL_INC=/app/openssl/include \
    SSL_LIB=/app/openssl \
    DEFINE="-DOPENSSL_SUPPRESS_DEPRECATED"

FROM alpine:edge

WORKDIR /usr/bin

RUN apk add --no-cache ca-certificates

COPY --from=builder /app/haproxy/haproxy haproxy

ENTRYPOINT ["haproxy"]

CMD ["-f", "/etc/haproxy/haproxy.cfg"]
