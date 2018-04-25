# easy-docker-redis-sentinel
简单实现使用 docker与docker-compose 实现redis 哨兵模式的集群部署

### 配置文件讲解
1.在docker-compose中建立一个master 直接使用官方提供的镜像即可<br>
2.在docker-compose中建立一个slave  使用官方镜像，覆盖官方执行命令（主要设置随从模式 slaveof） redis-server --slaveof redis-master 6379<br>
  将容器与master容器建立连接，注意连接时需要使用别名，此版本为 redis-master<br>
3.在docker-compose中建立一个sentinel，构建主要是将配置文件copy到容器中，当然也可以使用数据卷的方式构建conf <br>
  注：文件中的master连接别名为 redis-master 不要随意更改docker-compose文件中的别名，否则配置文件也需要一起更改。<br>
### 启动,横向扩展哨兵与从服务
```
# docker-compose up
# docker-compose scale redis-slave=2
# docker-compose sclae redis-sentinel=2
```
### 验证主从
```
# docker exec -it redis-cluster_redis-master_1 redis-cli 
# set zhaoyang yes
# exit
# docker exec -it redis-cluster_redis-slave_1 redis-cli
# get zhaoyang
```

### 验证哨兵
```
# docker exec -it redis-cluster_redis-sentinel_1 redis-cli -p 26379
# sentinel master mymaster
```
### 验证master宕机自动选举
```
# docker stop  redis-cluster_redis-master_1 
# docker-compose logs -f
```
在日志可以看到的具体的ip，日志中看到的ip是由docker分配出来的ip。






