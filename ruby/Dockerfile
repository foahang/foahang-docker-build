FROM linux:1.0

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        bzip2 \
        ca-certificates \
        libffi-dev \
        libgmp-dev \
        libssl-dev \
        libyaml-dev \
        procps \
        zlib1g-dev \
    ; \
    rm -rf /var/lib/apt/lists/*

# skip installing gem documentation
RUN set -eux; \
    mkdir -p /usr/local/etc; \
    { \
        echo 'install: --no-document'; \
        echo 'update: --no-document'; \
    } >> /usr/local/etc/gemrc

ENV LANG C.UTF-8

# Ruby version to be installed
ENV RUBY_VERSION 3.3.6
ENV RUBY_DOWNLOAD_URL http://192.168.189.224:9000/soft/linux/common/ruby/ruby-3.3.6.tar.xz

RUN set -eux; \
    \
    savedAptMark="$(apt-mark showmanual)"; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        dpkg-dev \
        libgdbm-dev \
        ruby \
        autoconf \
        g++ \
        gcc \
        libbz2-dev \
        libgdbm-compat-dev \
        libglib2.0-dev \
        libncurses-dev \
        libxml2-dev \
        libxslt-dev \
        make \
        wget \
        xz-utils \
    ; \
    rm -rf /var/lib/apt/lists/*; \
    \
    mkdir -p /tmp/rust; \
    wget -O /tmp/rust/rustup-init 'http://192.168.189.224:9000/soft/linux/common/ruby/rustup-init'; \
    chmod +x /tmp/rust/rustup-init; \
    export RUSTUP_HOME='/tmp/rust/rustup' CARGO_HOME='/tmp/rust/cargo'; \
    export PATH="$CARGO_HOME/bin:$PATH"; \
    env RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup /tmp/rust/rustup-init -y --no-modify-path --profile minimal --default-toolchain stable --default-host 'x86_64-unknown-linux-gnu'; \
    rustc --version; \
    cargo --version; \
    \
    wget -O ruby.tar.xz "$RUBY_DOWNLOAD_URL"; \
    \
    mkdir -p /usr/src/ruby; \
    tar -xJf ruby.tar.xz -C /usr/src/ruby --strip-components=1; \
    rm ruby.tar.xz; \
    \
    cd /usr/src/ruby; \
    \
    { \
        echo '#define ENABLE_PATH_CHECK 0'; \
        echo; \
        cat file.c; \
    } > file.c.new; \
    mv file.c.new file.c; \
    \
    autoconf; \
    gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
    ./configure \
        --build="$gnuArch" \
        --disable-install-doc \
        --enable-shared \
        --enable-yjit \
    ; \
    make -j "$(nproc)"; \
    make install; \
    \
    rm -rf /tmp/rust; \
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark > /dev/null; \
    find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' \
        | awk '/=>/ { so = $(NF-1); if (index(so, "/usr/local/") == 1) { next }; gsub("^/(usr/)?", "", so); printf "*%s\n", so }' \
        | sort -u \
        | xargs -r dpkg-query --search \
        | cut -d: -f1 \
        | sort -u \
        | xargs -r apt-mark manual \
    ; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    \
    cd /; \
    rm -r /usr/src/ruby; \
    if dpkg -l | grep -i ruby; then exit 1; fi; \
    [ "$(command -v ruby)" = '/usr/local/bin/ruby' ]; \
    ruby --version; \
    gem --version; \
    bundle --version

# 使用国内镜像
RUN set -eux; \
    gem sources --add https://mirrors.tuna.tsinghua.edu.cn/rubygems/ --remove https://rubygems.org/; \
    bundle config mirror.https://rubygems.org https://mirrors.tuna.tsinghua.edu.cn/rubygems

ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_SILENCE_ROOT_WARNING=1 \
    BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $GEM_HOME/bin:$PATH
RUN set -eux; \
    mkdir "$GEM_HOME"; \
    chmod 1777 "$GEM_HOME"

CMD [ "irb" ]