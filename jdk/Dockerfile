FROM linux:1.0

# 定义变量
ARG JAVA_VERSION=17.0.13_11
ARG JAVA_TAR=OpenJDK17U-jdk_x64_linux_hotspot_${JAVA_VERSION}.tar.gz
ARG JAVA_HOME=/opt/java/openjdk

# 设置非交互式安装
ENV DEBIAN_FRONTEND=noninteractive

# 下载并安装 JDK
RUN set -eux \
    # 创建目录并下载解压 JDK
    && mkdir -p ${JAVA_HOME} \
    && curl -L http://foahang.top:5205/file/linux/common/${JAVA_TAR} -o /tmp/${JAVA_TAR} \
    && tar -xzf /tmp/${JAVA_TAR} -C ${JAVA_HOME} --strip-components=1 \
    # 清理
    && rm -rf /tmp/${JAVA_TAR} \
    && rm -rf /tmp/* /var/tmp/* \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    # 设置环境变量
    && echo "export JAVA_HOME=${JAVA_HOME}" >> /etc/profile.d/java.sh \
    && echo "export PATH=${JAVA_HOME}/bin:\$PATH" >> /etc/profile.d/java.sh

ENV JAVA_HOME=${JAVA_HOME} \
    PATH=${JAVA_HOME}/bin:$PATH
