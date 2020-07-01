## docker基础知识
[Docker 技术从入门到实战](https://yeasy.gitbook.io/docker_practice/)
## [redis](./redis/README.md)

## [mysql](./mysql/READEME.md)

## [php](./php/README.md)

## [nginx](./nginx/README.md)

## docker-compose命令
切换到docker-compose.yml文件所在位置
```shell
#每次修改docker-compose.yml或者Dockerfile都要重新build
docker-compose build
#非daemon启动
docker-compose up
#daemon启动
docker-compose up -d
#停止
docker-compose down
```

## 单独构建的container之间要互相访问

要互相访问的container，加入同一个网段即可

1. 新建`mynet`(docker network ls显示的自定义名字)，自定义未被使用的网段，docker默认使用172.17.0.1网段

   ```shell
   docker network create --subnet 172.18.0.1/16 mynet
   ```
2. 把要互相访问的container加入新建的mynet

   比如container1，container2加入mynet，container1就可以访问container2了

   ```shell
   docker network connect mynet container1
   docker network connect mynet container2
   ```

   查看container对应的ip，使用ip访问

   ```shell
   docker container inspect containerName
   ```

   