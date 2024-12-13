FROM linux:1.0

# 设置非交互式安装
ENV DEBIAN_FRONTEND=noninteractive

# 设置Gitea工作目录
ENV GITEA_WORK_DIR=/var/lib/gitea/

# 定义变量
ARG GITEA_VERSION=1.22.1
ARG GITEA_BINARY=gitea-${GITEA_VERSION}-linux-amd64

# 下载并安装Gitea
RUN wget -O /usr/local/bin/gitea http://foahang.top:5205/file/linux/common/${GITEA_BINARY} \
    && chmod +x /usr/local/bin/gitea

# 创建git用户和组
RUN groupadd --system --gid 400 git && \
    useradd --system \
    --no-create-home \
    --home-dir /home/git \
    --shell /bin/bash \
    --uid 400 \
    --gid git \
    git

# 创建必要的目录
RUN mkdir -p /var/lib/gitea/{custom,data,log} /etc/gitea /home/git \
    && chown -R git:git /var/lib/gitea /home/git \
    && chmod -R 750 /var/lib/gitea \
    && chown root:git /etc/gitea \
    && chmod 770 /etc/gitea

# 设置工作目录
WORKDIR /var/lib/gitea

# 暴露端口
EXPOSE 3000 22

VOLUME ["/var/lib/gitea"]

# 切换到git用户
USER git

# 启动Gitea
CMD ["/usr/local/bin/gitea", "web", "-c", "/etc/gitea/app.ini"]
