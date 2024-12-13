FROM linux:1.0

# 设置工作目录
WORKDIR /cloudreve

# 从指定URL下载并解压主程序到 /cloudreve
RUN wget http://192.168.189.224:9000/soft/linux/common/cloudreve_3.8.3_linux_amd64.tar.gz -O cloudreve.tar.gz && \
    tar -zxvf cloudreve.tar.gz && \
    chmod +x cloudreve && \
    rm cloudreve.tar.gz

# 定义数据卷
VOLUME ["/cloudreve/uploads", "/cloudreve/avatar"]

# 暴露端口并启动 Cloudreve
EXPOSE 5212
CMD ["/cloudreve/cloudreve", "-c", "/cloudreve/config.ini"]
