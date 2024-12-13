仓库：https://nginx.org/packages/ubuntu/

# 快捷命令

docker run --name nginx -d -p 8070:80 nginx:1.0

docker run --rm --entrypoint=cat nginx:1.0 /etc/nginx/nginx.conf > ./nginx.conf

docker run --name nginx -v ./nginx.conf:/etc/nginx/nginx.conf:ro -d nginx:1.0

# 关键目录

/usr/share/nginx/html