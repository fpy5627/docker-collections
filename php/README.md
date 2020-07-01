选择不同php版本，可以修改docker-compose.yml build的来源

需要新增php扩展，修改Dockerfile
添加 docker-php-ext-install ${extension name}

## 启动
```shell
docker-compose up -d 
```