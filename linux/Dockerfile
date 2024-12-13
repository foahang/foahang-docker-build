FROM ubuntu:24.04

# 设置 hosts 文件
COPY hosts /etc/hosts
# 将 ubuntu.sources 文件复制到 Docker 镜像中
COPY ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources

# RUN set -eux \
#     && echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ noble main restricted universe multiverse" > /etc/apt/sources.list \
#     && echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ noble-updates main restricted universe multiverse" >> /etc/apt/sources.list \
#     && echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ noble-backports main restricted universe multiverse" >> /etc/apt/sources.list \
#     && echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ noble-security main restricted universe multiverse" >> /etc/apt/sources.list

# 删除默认的 sources.list 并更新软件包
RUN set -eux \
    && rm -f /etc/apt/sources.list \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/*

# 安装必要的工具包
RUN set -eux \
    && apt-get update \
    && apt-get install -y \
        ca-certificates \
        unzip \
        wget \
        curl \
        tzdata \
        vim \
        git \
        tar \
        gzip \
        iputils-ping \
        sshpass \
        locales \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/*

# 设置时区和语言环境
RUN set -eux \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && localedef -i zh_CN -c -f UTF-8 -A /usr/share/locale/locale.alias zh_CN.UTF-8

# 设置默认的环境变量
ENV LANG=zh_CN.UTF-8 \
    LC_ALL=zh_CN.UTF-8