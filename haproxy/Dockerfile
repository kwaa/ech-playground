FROM alpine:edge as builder

WORKDIR /app

RUN set -ex; \
    apk add --no-cache --virtual deps \
    build-base \
    ca-certificates \
    git \
    linux-headers \
    perl \
    # https://github.com/docker-library/haproxy/blob/master/2.5/alpine/Dockerfile
    lua5.4-dev \
    pcre2-dev \
    readline-dev; \
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
    USE_GETADDRINFO=1 \
    USE_LUA=1 \
    LUA_INC=/usr/include/lua5.4 \
    LUA_LIB=/usr/lib/lua5.4 \
    USE_PCRE2=1 \
    USE_PCRE2_JIT=1 \
    USE_PROMEX=1 \
    USE_OPENSSL=1 \
    SSL_INC=/app/openssl/include \
    SSL_LIB=/app/openssl \
    DEFINE="-DOPENSSL_SUPPRESS_DEPRECATED"

FROM alpine:edge

WORKDIR /usr/bin

RUN apk add --no-cache \
    ca-certificates \
    # https://pkgs.alpinelinux.org/package/edge/main/x86/haproxy
    busybox-binsh \
    libcrypto3 \
    libssl3 \
    lua5.4-libs \
    musl \
    pcre \
    zlib

COPY --from=builder /app/haproxy/haproxy haproxy

ENTRYPOINT ["haproxy"]

CMD ["-f", "/etc/haproxy/haproxy.cfg"]
