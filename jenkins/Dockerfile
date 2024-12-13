FROM jdk:1.0

ARG http_port=8080
ARG agent_port=50000
ARG JENKINS_HOME=/var/jenkins_home
ARG REF=/usr/share/jenkins/ref

ENV JENKINS_HOME=$JENKINS_HOME
ENV JENKINS_SLAVE_AGENT_PORT=${agent_port}
ENV REF=$REF

# 下载jenkins.war和jenkins-plugin-manager
RUN mkdir -p /usr/share/jenkins && \
    curl -L http://192.168.189.224:9000/soft/linux/common/jenkins/jenkins.war -o /usr/share/jenkins/jenkins.war && \
    curl -L http://192.168.189.224:9000/soft/linux/common/jenkins/jenkins-plugin-manager-2.13.1.jar -o /opt/jenkins-plugin-manager.jar

# 安装字体和其他必要的包
RUN apt-get update && \
    apt-get install -y fontconfig freetype* tini && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 创建必要的目录
RUN mkdir -p $JENKINS_HOME \
    && mkdir -p ${REF}/init.groovy.d

# 设置环境变量，使用国内的插件镜像地址
ENV JENKINS_UC=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates
ENV JENKINS_UC_EXPERIMENTAL=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/experimental
ENV JENKINS_INCREMENTALS_REPO_MIRROR=https://mirrors.tuna.tsinghua.edu.cn/jenkins/incrementals

# 暴露端口
EXPOSE ${http_port}
EXPOSE ${agent_port}

ENV COPY_REFERENCE_FILE_LOG=$JENKINS_HOME/copy_reference_file.log

VOLUME $JENKINS_HOME

# 复制必要的脚本
COPY jenkins-support /usr/local/bin/jenkins-support
COPY jenkins.sh /usr/local/bin/jenkins.sh
COPY jenkins-plugin-cli.sh /bin/jenkins-plugin-cli

# 添加执行权限
RUN chmod +x /usr/local/bin/jenkins.sh /usr/local/bin/jenkins-support /bin/jenkins-plugin-cli

# 设置入口点
ENTRYPOINT ["/usr/bin/tini", "--", "/usr/local/bin/jenkins.sh"]
