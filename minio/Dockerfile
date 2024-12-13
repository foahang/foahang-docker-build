FROM linux:1.0

# 下载 MinIO 二进制文件
RUN curl -L http://foahang.top:5205/file/linux/common/minio -o /usr/local/bin/minio \
    && chmod +x /usr/local/bin/minio

# 创建数据目录和配置目录
RUN mkdir -p /data /config

# 设置工作目录
WORKDIR /data

# 暴露 /data 和 /config 目录
VOLUME ["/data", "/config"]

# 暴露 MinIO 默认端口
EXPOSE 9000 9001

# 设置环境变量
ENV MINIO_ROOT_USER=minioadmin
ENV MINIO_ROOT_PASSWORD=minioadmin
ENV MINIO_CONFIG_DIR=/config

# 设置启动命令
CMD ["minio", "server", "/data", "--console-address", ":9001", "--address", ":9000", "--config-dir", "/config"]
