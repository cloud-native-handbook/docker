# Docker 命令

## 容器

```sh
$ docker ps # 正在运行的容器
$ docker ps -a # 所有容器
$ docker ps -a | grep 'Exited' # 退出的容器
$ docker inspect [container] # 容器信息
$ docker rename [container] [new_name] # 修改名称
$ docker rm -f -v [container] # 删除容器，-f：强制删除，无论是否在运行，-v：删除自动挂载的 volume（volume name是随机生成的）， 如果是手动挂载的 volume 不会被删除
```

```sh
# 运行容器
# -it： 交互模式
# --net: 使用的网络, --net 等同于 --network
# -p: 主机与容器的端口映射（-p 8080:8080 或 -p 192.168.1.2:8080:8080）， -p 等同于 --public
# -P: 把容器的端口随机映射到主机，这里容器端口指的是 Dockerfile EXPOSE 出来的端口
# -e： 环境变量, -e 等同于 --env
# --add-host：添加主机和IP的映射（--add-host centos192:192.168.1.192， 会被添加到容器的 /etc/hosts）, 一次只添加一个
# --dns: 添加 dns 到容器的 /etc/resolv.conf，一次只添加一个
# -d： 后台运行， -d 等同于 --detach
# -v 挂载（针对目录而言）， 挂载到主机：-v /var/lib/mysql:/data/mysql，挂载到卷： -v mysql_data:/data/mysql
# --entrypoint
$ docker run -it --name [container] --net [network] --ip [ip] --hostname [host] -p [port:port] -e [ENV=...] --add-host [host:ip] --dns [8.8.8.8] -v [volume:/container/path] --entrypoint [cmd] -d [image:tag]
```

```sh
$ docker exec -it [container] bash # 进入容器并指定 bash 命令
```

```sh
$ docker export [container] [xxx.tar] # 导出容器文件系统为 tar 包
$ docker import [xxx.tar] # 导入 tar 包创建文件系统镜像
$ docker commit [container] [image:tar] # 将容器的修改创建成镜像
```

```sh
# 在主机和容器之间 copy 数据
$ docker cp /root/docker-entrypoint.sh mysql:/usr/local/bin/ # 主机 -> 容器
$ docker cp mysql:/usr/local/bin/docker-entrypoint.sh /root/ # 容器 -> 主机
```

```sh
$ docker top [container] # 查看容器中正在运行的进程
$ docker diff [container] # 查看容器内发生变化的文件
$ docker events # 实时输出服务端事件，包括容器的创建，启动，关闭等
$ docker logs (-f) [container] # 容器日志
```

Expose UDP 端口：

```sh
$ docker run -p 8080:8080/udp .....
```

与此同时，要求 Dockerfile 也应该（但不是必须的） Expose UDP 端口：

```Dockerfile
EXPOSE 8080/udp
```

## 镜像

```sh
$ docker images # 所有镜像
$ docker images | grep 'none' # 空镜像
$ docker history [image] # 镜像历史
$ docker tag [image:tag] [new_image:tag] # 重命名镜像
$ docker rmi [image] # 删除镜像
```

```sh
$ docker save -o nginx-1.11.9-alpine.tar nginx:1.11.9-alpine # 打包镜像成 tar 包
$ docker load -i nginx-1.11.9-alpine.tar # 从 tar 包中加载镜像
```
