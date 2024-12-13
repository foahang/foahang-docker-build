FROM linux:1.0

# 设置 nginx 相关的环境变量
ENV NGINX_VERSION=1.26.2
ENV NJS_VERSION=0.8.5
ENV NJS_RELEASE=1~noble
ENV PKG_RELEASE=1~noble
ENV DYNPKG_RELEASE=2~noble

# 执行一系列命令来安装和配置 nginx
RUN set -x \
    # 创建 nginx 用户和用户组,用于运行 nginx 进程
    && groupadd --system --gid 5201 nginx \
    && useradd --system --gid nginx --no-create-home --home /nonexistent --comment "nginx user" --shell /bin/false --uid 5201 nginx \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates \
    # 定义要安装的 nginx 包列表
    && nginxPackages=" \
        nginx=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-xslt=${NGINX_VERSION}-${DYNPKG_RELEASE} \
        nginx-module-geoip=${NGINX_VERSION}-${DYNPKG_RELEASE} \
        nginx-module-image-filter=${NGINX_VERSION}-${DYNPKG_RELEASE} \
        nginx-module-njs=${NGINX_VERSION}+${NJS_VERSION}-${NJS_RELEASE} \
    " \
    # 从仓库安装 nginx
    && echo "deb [signed-by=/dev/null trusted=yes] http://dev:I5uGVWSiJ7tZuPc02ClO@foahang.top:5203/repository/apt-ubuntu-noble-nginx/ noble nginx" >> /etc/apt/sources.list.d/nginx.list \
    && apt-get update \
    # 安装 nginx 包和其他必要工具
    && apt-get install --no-install-recommends --no-install-suggests -y \
                        $nginxPackages \
                        gettext-base \
                        curl \
    # 清理安装缓存和临时文件
    && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx.list \
    # 配置日志输出到 docker 日志收集器
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# 暴露 80 端口
EXPOSE 80

# 设置停止信号为 SIGQUIT
STOPSIGNAL SIGQUIT

# 设置容器启动时执行的命令,以非守护进程方式启动 nginx
CMD ["nginx", "-g", "daemon off;"]