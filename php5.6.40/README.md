- php:5.6.40
- phalcon-v2.0.7
- phpredis-2.2.7
- xdebug-2.5.5
- pdo_mysql pdo
- gd
- zip

```
docker build -t menzai/php56-phalcon2-redis2-xdebug2:v1.x .

# 走宿主机代理的话（macOS或Windows上用host.docker.internal代表宿主机，Linux上用 172.17.0.1）
docker build --build-arg HTTP_PROXY=http://host.docker.internal:7890 -t menzai/php56-phalcon2-redis2-xdebug2:v1.x .

# 多阶构建的Dockerfile
docker build -f Dockerfile-multi-stage-builds --build-arg HTTP_PROXY=http://host.docker.internal:7890 -t menzai/php56-phalcon2-redis2-xdebug2:v1.x .
```