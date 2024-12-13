FROM linux:1.0

# 创建mysql用户和用户组
RUN set -eux; \
	groupadd --system --gid 300 mysql; \
	useradd --system --uid 300 --gid 300 --home-dir /var/lib/mysql --no-create-home mysql

# 添加gosu用于从root用户降权
RUN set -eux; \
	wget -O /usr/local/bin/gosu "http://foahang.top:5205/file/linux/common/gosu-amd64"; \
	chmod +x /usr/local/bin/gosu; \
	gosu --version; \
	gosu nobody true

# 创建初始化脚本目录
RUN mkdir /docker-entrypoint-initdb.d

# 设置MySQL版本环境变量
ENV MYSQL_MAJOR=8.0
ENV MYSQL_VERSION=8.0.40-1ubuntu24.04

# 添加MySQL APT源
RUN echo 'deb [signed-by=/dev/null trusted=yes] http://dev:I5uGVWSiJ7tZuPc02ClO@foahang.top:5203/repository/apt-ubuntu-noble-mysql/ noble mysql-8.0' > /etc/apt/sources.list.d/mysql.list

# 配置MySQL安装选项并安装MySQL
# /var/lib/mysql相关配置是因为mysql-server postinst没有显式方法禁用mysql_install_db代码路径
# 同时设置debconf键以使APT更安静
RUN { \
		echo mysql-community-server mysql-community-server/data-dir select ''; \
		echo mysql-community-server mysql-community-server/root-pass password ''; \
		echo mysql-community-server mysql-community-server/re-root-pass password ''; \
		echo mysql-community-server mysql-community-server/remove-test-db select false; \
	} | debconf-set-selections \
	&& apt-get update \
	&& apt-get install -y \
		mysql-community-client="${MYSQL_VERSION}" \
		mysql-community-server-core="${MYSQL_VERSION}" \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql /var/run/mysqld \
	&& chown -R mysql:mysql /var/lib/mysql /var/run/mysqld \
# 确保/var/run/mysqld(用于socket和锁文件)可写,不管运行时mysqld实例最终使用什么UID
	&& chmod 1777 /var/run/mysqld /var/lib/mysql

# 声明数据卷
VOLUME /var/lib/mysql

# 复制配置文件
COPY config/ /etc/mysql/
COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh && \
    ln -s /usr/local/bin/docker-entrypoint.sh /entrypoint.sh # 向后兼容

# 设置入口点
ENTRYPOINT ["docker-entrypoint.sh"]

# 暴露端口
EXPOSE 3306 33060
CMD ["mysqld"]