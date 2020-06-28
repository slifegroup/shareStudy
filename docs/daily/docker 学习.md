1. docker 主要一些命令

   * docker image 查看镜像

   * docker pull image:tag (带上标签版本) 拉取镜像

   * docker ps 查看运行的容器

   * docker rm 容器名 删除容器

   * docker stop/ start/ restart  容器的编号/容器名 

     停止/启动/重启 容器的运行

   * docker rename CONTAINER NEW_NAME

     容器重命名

   * docker run -t  -i  image:tag  /bin/bash

     参数说明：

     * **-i**: 交互式操作。
     * **-t**: 终端。
     * **ubuntu:15.10**: 这是指用 ubuntu 15.10 版本镜像为基础来启动容器。
     * **/bin/bash**：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash

   * docker attach  容器编号  进入容器内部

     ```
     docker exec -it postgres-dev(容器名) bash
     exit 退出容器
     ```

2. docker 安装 mysql

   ```
   docker pull mysql:5.7.30
   docker run -itd --name 'mysql-dev' -p 3307:3306 -e MYSQL_ROOT_PASSWORD=rjgf20200612%  mysql:5.7.30
   ```

3. docker 安装 pgsql (https://www.infoq.cn/article/iq7QwGsxR60VFowl3KsW)

   ```
   docker pull postgres:9.6.15
   docker run --name postgres-dev -e POSTGRES_PASSWORD=rjgf20200612% -p 54322:5432 -d postgres:9.6.15
   ```

4. docker 安装 redis

   ```
   docker pull redis
   docker run --name redis-product -d redis
   ```

   

5. docker 添加阿里加速器

   ```shell
   sudo mkdir -p /etc/docker
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://ofrfxjgf.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

6. docker daemon 开启远程调用，让客户端调用

   主要原因：**docker操作命令只能是root用户或者属于docker组中用户才能有对应的操作权限** 

   ```
   dockerd 
   -H 0.0.0.0:2375 // window 调用 tcp://ip:2375
   -H unix:///var/run/docker.sock // uinx 调用
   ```

7. docker 远程仓库/注册中心 (存放一些私有的镜像)

   1. 修改tag

      ```shell
      [root@localhost docker-test]# docker tag xula/docker-test:1.0.0 localhost/docker-test:1.0.0
      ```

   2. 推送到本地仓库

      ```shell
      [root@localhost docker-test]# docker push localhost:5000/docker-test:1.0.0
      ```

   3. 查看仓库信息

      ```
      [root@localhost docker-test]# curl http://localhost:5000/v2/_catalog
      {"repositories":["docker-test"]}
      ```

      

