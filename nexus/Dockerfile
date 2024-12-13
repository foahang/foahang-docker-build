FROM jdk:1.0

ARG NEXUS_VERSION=3.72.0-04

# 配置Nexus运行时环境
ENV SONATYPE_DIR=/opt/sonatype
ENV NEXUS_HOME=${SONATYPE_DIR}/nexus \
    NEXUS_DATA=/nexus-data \
    NEXUS_CONTEXT='' \
    SONATYPE_WORK=${SONATYPE_DIR}/sonatype-work

# 在Ubuntu中创建用户和组
RUN groupadd --gid 200 -r nexus \
    && useradd --uid 200 -r nexus -g nexus -s /usr/sbin/nologin -d /opt/sonatype/nexus -c 'Nexus Repository Manager user'

WORKDIR ${SONATYPE_DIR}

# 下载并设置Nexus
RUN curl -L http://foahang.top:5205/file/linux/common/nexus-${NEXUS_VERSION}-unix.tar.gz -o nexus-${NEXUS_VERSION}-unix.tar.gz \
    && tar -xvf nexus-${NEXUS_VERSION}-unix.tar.gz \
    && rm -f nexus-${NEXUS_VERSION}-unix.tar.gz \
    && mv nexus-${NEXUS_VERSION} $NEXUS_HOME \
    && mkdir -p ${SONATYPE_WORK} \
    && chown -R nexus:nexus ${SONATYPE_WORK} \
    && mkdir -p ${NEXUS_DATA} \
    && chown -R nexus:nexus ${NEXUS_DATA} \
    && mv ${SONATYPE_WORK}/nexus3 ${NEXUS_DATA} \
    && ln -s ${NEXUS_DATA} ${SONATYPE_WORK}/nexus3

# 从nexus.vmoptions中移除Java内存设置
RUN sed -i '/^-Xms/d;/^-Xmx/d;/^-XX:MaxDirectMemorySize/d' $NEXUS_HOME/bin/nexus.vmoptions

RUN echo "#!/bin/bash" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
   && echo "cd /opt/sonatype/nexus" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
   && echo "exec ./bin/nexus run" >> ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
   && chmod a+x ${SONATYPE_DIR}/start-nexus-repository-manager.sh \
   && sed -e '/^nexus-context/ s:$:${NEXUS_CONTEXT}:' -i ${NEXUS_HOME}/etc/nexus-default.properties

# 修改INSTALL4J_JAVA_HOME_OVERRIDE的值
RUN sed -i 's|^# INSTALL4J_JAVA_HOME_OVERRIDE=.*|INSTALL4J_JAVA_HOME_OVERRIDE=/usr/lib/jvm/java-17-openjdk-amd64|' ${NEXUS_HOME}/bin/nexus

VOLUME ${NEXUS_DATA}

EXPOSE 8081
USER nexus

ENV INSTALL4J_ADD_VM_PARAMS="-Xms2703m -Xmx2703m -XX:MaxDirectMemorySize=2703m -Djava.util.prefs.userRoot=${NEXUS_DATA}/javaprefs"

CMD ["/opt/sonatype/nexus/bin/nexus", "run"]
