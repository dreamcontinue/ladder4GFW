## [利用Docker安装NextCloud](https://www.moerats.com/archives/420/)

安装Docker
``` shell script
curl -sSL https://get.docker.com/ | sh
systemctl start docker
systemctl enable docker
```

数据库镜像
 ```shell script
docker run --name mysqlnc -d \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=EXAMPLE \
-e MYSQL_DATABASE=EXAMPLE \
-e MYSQL_USER=EXAMPLE \
-e MYSQL_PASSWORD=EXAMPLE \
-v /root/nextcloud/mysql:/var/lib/mysql \
mysql:5.7
 ```

云盘镜像
 ```shell script
docker run -d --name nextcloud --link mysqlnc \
-v /root/nextcloud/data:/data \
-p 3000:80 \
rootlogin/nextcloud
 ```

开启3000端口

域名反代：

手动修改
 ```shell script
echo "xx.com {
 gzip
 proxy / 127.0.0.1:3000 {
    header_upstream Host {host}
    header_upstream X-Real-IP {remote}
    header_upstream X-Forwarded-For {remote}
    header_upstream X-Forwarded-Port {server_port}
    header_upstream X-Forwarded-Proto {scheme}
  }
}" > /usr/local/caddy/Caddyfile
 ```

or

 ```shell script

#https
echo "xx.com {
 gzip
 tls admin@moerats.com
 proxy / 127.0.0.1:3000 {
    header_upstream Host {host}
    header_upstream X-Real-IP {remote}
    header_upstream X-Forwarded-For {remote}
    header_upstream X-Forwarded-Port {server_port}
    header_upstream X-Forwarded-Proto {scheme}
  }
}" > /usr/local/caddy/Caddyfile
 ```

重启caddy