FROM alpine:3.8

ENV NODE_VERSION=8.9.1 \
    RUBY_BASE_VERSION=2.3 \
    RUBY_VERSION=2.3.0 \
    RUBY_BUNDLER_VERSION=1.15.4 \
    CFLAGS="-O2" \
    LDFLAGS="-fuse-ld=gold"

RUN apk add --update --no-cache \
        libstdc++ \
    && apk add --no-cache --virtual .build-deps \
        binutils-gold \
        build-base \
        curl \
        gnupg \
        linux-headers \
        python \
        tar \
        xz \
    && for key in \
      94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
      FD3A5288F042B6850C66B31F09FE44734EB7990E \
      71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
      DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
      C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
      B9AE9905FFD7803F25714661B63B535A4C206CA9 \
      56730D5401028683275BD23C23EFEFE93C4CFFFE \
      77984A986EBC2AA786BC0F66B01FBB92821C587A \
      8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
      ; do \
        gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
        gpg --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
        gpg --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
      done \
    && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.xz" \
    && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xf "node-v$NODE_VERSION.tar.xz" \
    && cd "node-v$NODE_VERSION" \
    && ./configure \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install \
    && apk del .build-deps \
    && cd .. \
    && rm -Rf "node-v$NODE_VERSION" \
    && rm "node-v$NODE_VERSION.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt

COPY include-ruby-missing.h.patch /

RUN apk add --no-cache \
        jemalloc libffi libssl1.0 libxml2 libxslt readline yaml \
    && apk add --no-cache --virtual .build-deps \
        bash binutils-gold build-base git jemalloc-dev  \
        libffi-dev libxml2-dev libxslt-dev linux-headers \
        openssl-dev readline-dev wget yaml-dev \
    && wget http://ftp.ruby-lang.org/pub/ruby/$RUBY_BASE_VERSION/ruby-$RUBY_VERSION.tar.gz \
    && tar -xzvf ruby-$RUBY_VERSION.tar.gz \
    && patch ruby-$RUBY_VERSION/include/ruby/missing.h < /include-ruby-missing.h.patch \
    && cd ruby-$RUBY_VERSION \
    && ./configure --disable-install-doc --disable-install-rdoc --disable-install-capi --with-jemalloc \
    && make -j4 \
    && make install \
    && cd / \
    && rm -fr ruby-$RUBY_VERSION ruby-$RUBY_VERSION.tar.gz include-ruby-missing.h.patch \
    && apk del .build-deps
