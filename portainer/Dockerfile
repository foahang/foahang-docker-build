FROM linux:1.0

# 定义变量
ARG PORTAINER_VERSION=2.21.4
ARG PORTAINER_TAR=portainer-${PORTAINER_VERSION}-linux-amd64.tar.gz

# 设置非交互式安装
ENV DEBIAN_FRONTEND=noninteractive

# 下载并安装 Portainer
RUN set -eux \
    # 创建目录并下载解压 Portainer
    && mkdir -p /opt/portainer \
    && curl -L http://foahang.top:5205/file/linux/common/${PORTAINER_TAR} -o /tmp/${PORTAINER_TAR} \
    && tar -xzf /tmp/${PORTAINER_TAR} -C /opt/portainer --strip-components=1 \
    # 清理
    && rm -rf /tmp/${PORTAINER_TAR} \
    && rm -rf /tmp/* /var/tmp/* 

# 设置数据卷
VOLUME /data

# 暴露Portainer端口
EXPOSE 9000 9443

# 设置工作目录
WORKDIR /opt/portainer

# 启动Portainer
CMD ["/opt/portainer/portainer"]
