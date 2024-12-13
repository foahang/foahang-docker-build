FROM linux:1.0

# 安装编译所需的依赖
RUN set -eux \
    && apt-get update && apt-get install -y \
        build-essential \
        gcc \
        libc-dev \
        make \
        openssl \
        pkg-config \
        libpcre3 \
        libpcre3-dev \
        zlib1g-dev \
        libxslt1-dev \
        libgd-dev \
        libgeoip-dev \
        libssl-dev \ 
        gnupg1 \
        libperl-dev \
        git \
    # 清理apt缓存和临时文件
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/*

# 设置工作目录
WORKDIR /src
