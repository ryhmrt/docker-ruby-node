FROM ubuntu:bionic

ENV NODE_VERSION=v8.9.1 \
    NODE_DISTRO=linux-x64 \
    RUBY_BASE_VERSION=2.3 \
    RUBY_VERSION=2.3.0 \
    RUBY_GEMS_VERSION=2.7.9 \
    CFLAGS="-O2" \
    LDFLAGS="-fuse-ld=gold"

RUN apt-get update \
    && apt-get install -y wget \
    && wget https://nodejs.org/dist/$NODE_VERSION/node-$NODE_VERSION-$NODE_DISTRO.tar.gz \
    && tar -xzvf node-$NODE_VERSION-$NODE_DISTRO.tar.gz \
    && mkdir /usr/local/lib/nodejs \
    && mv node-$NODE_VERSION-$NODE_DISTRO /usr/local/lib/nodejs \
    && ln -s /usr/local/lib/nodejs/node-$NODE_VERSION-$NODE_DISTRO/bin/node /usr/bin/node \
    && ln -s /usr/local/lib/nodejs/node-$NODE_VERSION-$NODE_DISTRO/bin/npm /usr/bin/npm \
    && ln -s /usr/local/lib/nodejs/node-$NODE_VERSION-$NODE_DISTRO/bin/npx /usr/bin/npx \
    && rm node-$NODE_VERSION-$NODE_DISTRO.tar.gz \
    && apt-get install -y libffi6 libgdbm5 libjemalloc1 libreadline7 libssl1.0.0 libxml2 libxslt1.1 libyaml-0-2 nkf \
    && apt-get install -y autoconf bison build-essential libffi-dev libgdbm-dev libjemalloc-dev libreadline-dev libssl1.0-dev libxml2-dev libxslt-dev libyaml-dev zlib1g-dev \
    && wget http://ftp.ruby-lang.org/pub/ruby/$RUBY_BASE_VERSION/ruby-$RUBY_VERSION.tar.gz \
    && tar -xzvf ruby-$RUBY_VERSION.tar.gz \
    && cd ruby-$RUBY_VERSION \
    && echo export CC=$(which gcc-6) \
    && ./configure --disable-install-doc --disable-install-rdoc --disable-install-capi --with-jemalloc \
    && make -j4 \
    && make install \
    && echo export -n CC \
    && cd / \
    && rm -fr ruby-$RUBY_VERSION ruby-$RUBY_VERSION.tar.gz \
    && gem update --system $RUBY_GEMS_VERSION \
    && echo "gem: --no-document" > ~/.gemrc \
    && apt-get remove -y autoconf bison build-essential libgdbm-dev libjemalloc-dev libreadline7-dev libssl-dev libyaml-dev zlib1g-dev \
    && apt-get remove -y wget \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*
