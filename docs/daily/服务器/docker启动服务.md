## docker 启动服务 

1. 创建 redis 容器 

```
docker run --name redis-product -d redis
```

2. 创建 pgsql 容器

```
docker run --name postgres-dev -e POSTGRES_PASSWORD=xxxxxx -p 54322:5432 -d postgres:9.6.15
docker start postgres-product  #运行容器
```

3. nacos (单机版 Mysql)

  进入 `//opt/docker_v/nacos/nacos-docker` 目录运行如下命令：

```
docker-compose -f example/standalone-mysql.yaml up
```

​	上述命令自动安装了mysql，所以无需再创建mysql容器，`mysql` 参数修改，进入 `opt/docker_v/nacos/nacos-docker/env/` 目录，打开`mysql.env` 或  `nacos-standlone-mysql.env` 文件

* 具体参数

  ```
  PREFER_HOST_MODE=hostname
  MODE=standalone
  SPRING_DATASOURCE_PLATFORM=mysql
  MYSQL_SERVICE_HOST=mysql
  MYSQL_SERVICE_DB_NAME=nacos_devtest
  MYSQL_SERVICE_PORT=3306
  MYSQL_SERVICE_USER=nacos
  MYSQL_SERVICE_PASSWORD=nacos
  ```

4. 安装 rocketMQ 

   - 安装 Namesrv

     - 获取最新的版本镜像

     ```
     docker pull rocketmqinc
     ```

     - 启动容器

     ```
     docker run -d -p 9876:9876 -v /opt/rocketmq/data/namesrv/logs:/root/logs -v /opt/rocketmq/data/namesrv/store:/root/store --name rmqnamesrv --restart=always -e "MAX_POSSIBLE_HEAP=100000000" rocketmqinc/rocketmq sh mqnamesrv 
     ```

   - 安装 broker 服务器

     * 创建 broker.conf 文件

       1. 在 `/opt/rocketmq/conf` 目录下创建 broker.conf 文件
       2. 在 broker.conf 中写入如下内容

       ```
       brokerClusterName = DefaultCluster
       brokerName = broker-a
       brokerId = 0
       deleteWhen = 04
       fileReservedTime = 48
       brokerRole = ASYNC_MASTER
       flushDiskType = ASYNC_FLUSH
       brokerIP1 = {本地外网 IP}
       ```

     * 启动容器

       ```
       docker run -d -p 10911:10911 -p 10909:10909 -v  /opt/rocketmq/data/broker/logs:/root/logs -v  /opt/rocketmq/data/broker/store:/root/store -v  /opt/rocketmq/conf/broker.conf:/opt/rocketmq-4.4.0/conf/broker.conf --name rmqbroker --restart=always --link rmqnamesrv:namesrv -e "NAMESRV_ADDR=namesrv:9876" -e "MAX_POSSIBLE_HEAP=200000000" rocketmqinc/rocketmq sh mqbroker -c /opt/rocketmq-4.4.0/conf/broker.conf 
       ```

   * 安装 rocketmq-console-ng

     * 拉取镜像

       ```
       docker pull styletang/rocketmq-console-ng
       ```

     * 运行容器

       ```
       docker run -e "JAVA_OPTS=-Drocketmq.namesrv.addr=192.168.93.130:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" -p 8080:8080 -t --restart=always styletang/rocketmq-console-ng
       ```

       

   

   





