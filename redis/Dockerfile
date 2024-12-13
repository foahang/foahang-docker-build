FROM linux:1.0

# 安装Redis
RUN set -eux \
    # 安装必要工具
    && apt-get update \
    && apt-get install -y lsb-release \
    # 添加Redis仓库
    && echo "deb [signed-by=/dev/null trusted=yes] http://server:8FFlNXlw8kj7@nexus:8081/repository/apt-ubuntu-noble-redis $(lsb_release -cs) main" > /etc/apt/sources.list.d/redis.list \
    # 安装Redis
    && apt-get update \
    && apt-get install -y redis \
    # 清理
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/* \
    # 删除Redis仓库
    && rm -f /etc/apt/sources.list.d/redis.list

# 暴露Redis端口 
EXPOSE 6379

# 设置启动命令
CMD ["redis-server"]
