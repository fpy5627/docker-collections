
需要修改docker-compose.yml文件内，对应volumes映射本地目录
## 启动
```shell
docker-compose up -d 
```

代码内配置文件
mysql
host: mydb
pwd: 12345678
redis
host: myredis

## [参考hyperf/hyperf-docker](https://github.com/hyperf/hyperf-docker)

###kafka
```shell
RUN apk add --no-cache librdkafka-dev \
&& pecl install rdkafka \
&& echo "extension=rdkafka.so" > /etc/php7/conf.d/rdkafka.ini
aerospike
# aerospike @see https://github.com/aerospike/aerospike-client-php/issues/24
RUN git clone https://gitlab.innotechx.com/liyibocheng/aerospike-c-client.git /tmp/aerospike-client-c \
&& ( \
    cd /tmp/aerospike-client-c \
    && make \
) \
&& export PREFIX=/tmp/aerospike-client-c/target/Linux-x86_64 \
&& export DOWNLOAD_C_CLIENT=0 \
&& git clone https://gitlab.innotechx.com/liyibocheng/aerospike-client-php.git /tmp/aerospike-client-php \
&& ( \
    cd /tmp/aerospike-client-php/src \
    && ./build.sh \
    && make install \
    && echo "extension=aerospike.so" > /etc/php7/conf.d/aerospike.ini \
    && echo "aerospike.udf.lua_user_path=/usr/local/aerospike/usr-lua" >> /etc/php7/conf.d/aerospike.ini \
)
```
### mongodb
```shell
RUN apk add --no-cache openssl-dev \
&& pecl install mongodb \
&& echo "extension=mongodb.so" > /etc/php7/conf.d/mongodb.ini
```

### protobuf
```shell
# mac protobuf: https://blog.csdn.net/JoeBlackzqq/article/details/83118248
RUN apk add --no-cache protobuf \
&& cd /tmp \
&& pecl install protobuf \
&& echo "extension=protobuf.so" > /etc/php7/conf.d/protobuf.ini
```

### swoole tracker
```shell
# download swoole tracker
ADD ./swoole-tracker-install.sh /tmp

RUN chmod +x /tmp/swoole-tracker-install.sh \
&& cd /tmp \
&& ./swoole-tracker-install.sh \
&& rm /tmp/swoole-tracker-install.sh \
# config
&& cp /tmp/swoole-tracker/swoole_tracker72.so /usr/lib/php7/modules/swoole_tracker72.so \
&& echo "extension=swoole_tracker72.so" > /etc/php7/conf.d/swoole-tracker.ini \
&& echo "apm.enable=1" >> /etc/php7/conf.d/swoole-tracker.ini \
&& echo "apm.sampling_rate=100" >> /etc/php7/conf.d/swoole-tracker.ini \
&& echo "apm.enable_memcheck=1" >> /etc/php7/conf.d/swoole-tracker.ini \
# launch
&& printf '#!/bin/sh\n/opt/swoole/script/php/swoole_php /opt/swoole/node-agent/src/node.php' > /opt/swoole/entrypoint.sh \
&& chmod 755 /opt/swoole/entrypoint.sh
```

### fix aliyun oss wrong charset
```shell
# fix aliyun oss wrong charset: https://github.com/aliyun/aliyun-oss-php-sdk/issues/101
RUN apk --no-cache --allow-untrusted --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ add gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php
```