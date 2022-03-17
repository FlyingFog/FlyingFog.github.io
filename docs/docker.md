## 权限问题

```shell
sudo groupadd docker
sudo usermod -aG docker $USER
sudo chmod 666 /var/run/docker.sock

```

## 配置文件

```
$ vim /etc/docker/daemon.json  
{  
    "registry-mirrors":  
        ["http://7e61f7f9.m.daocloud.io"],  
    "graph": "/new-path/docker" 
}
```

换源：

修改路径：

## 基本使用

##### 直接运行

```
docker run ubuntu /bin/echo "Hello world"
```

##### **运行交互式容器**

```
docker run -i -t ubuntu /bin/bash

exit
```

- **-t:** 在新容器内指定一个伪终端或终端。
- **-i:** 允许你对容器内的标准输入 (STDIN) 进行交互。
- 退出

##### 启动容器（后台模式）

启动

```
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
2b1b7a428627c51ab8810d541d759f072b4fc75487eed05812646b8534a2fe63
```

查看

```
docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
aa1b2e05a569   ubuntu    "/bin/sh -c 'while t…"   12 seconds ago   Up 11 seconds             exciting_ride
```

输出详情介绍：

**CONTAINER ID:** 容器 ID。

**IMAGE:** 使用的镜像。

**COMMAND:** 启动容器时运行的命令。

**CREATED:** 容器的创建时间。

**STATUS:** 容器状态。

状态有7种：

- created（已创建）
- restarting（重启中）
- running 或 Up（运行中）
- removing（迁移中）
- paused（暂停）
- exited（停止）
- dead（死亡）

**PORTS:** 容器的端口信息和使用的连接类型（tcp\udp）。

**NAMES:** 自动分配的容器名称。

查看日志

```
docker logs aa1b2e05a569
```

停止

```
docker stop aa1b2e05a569
aa1b2e05a569
```

## 容器使用

```
#启动容器
docker run -it ubuntu /bin/bash
exit
```

- **-i**: 交互式操作。
- **-t**: 终端。
- **ubuntu**: ubuntu 镜像。
- **/bin/bash**：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。

##### 启动已停止运行的容器

```
#查看所有的容器命令如下：
docker ps -a 

#使用 docker start 启动一个已停止的容器：
docker start b750bbbcfd88 

#后台运行
docker run -itd --name ubuntu-test ubuntu /bin/bash

#停止容器
停止容器的命令如下：
$ docker stop <容器 ID>
停止的容器可以通过 docker restart 重启：
$ docker restart <容器 ID>

#进入容器
在使用 -d 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：
docker attach
docker exec：推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。

docker attach 1e560fca3906 
注意： 如果从这个容器退出，会导致容器的停止。

docker exec -it 243c32535da7 /bin/bash
注意： 如果从这个容器退出，容器不会停止，这就是为什么推荐大家使用 docker exec 的原因。
```

##### 导入和导出

```
#导出容器

如果要导出本地某个容器，可以使用 docker export 命令。
$ docker export 1e560fca3906 > ubuntu.tar

#导入容器快照

可以使用 docker import 从容器快照文件中再导入为镜像，以下实例将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:
$ cat docker/ubuntu.tar | docker import - test/ubuntu:v1
通过指定 URL 或者某个目录来导入，例如：
docker import http://example.com/exampleimage.tgz example/imagerepo
```

##### 删除容器

```
删除容器使用 docker rm 命令：
$ docker rm -f 1e560fca3906
```

## 示例：web应用

##### 运行web应用

前面我们运行的容器并没有一些什么特别的用处。

接下来让我们尝试使用 docker 构建一个 web 应用程序。

我们将在docker容器中运行一个 Python Flask 应用来运行一个web应用。

```
runoob@runoob:~# docker pull training/webapp  # 载入镜像
runoob@runoob:~# docker run -d -P training/webapp python app.py
```

参数说明:

- **-d:**让容器在后台运行。
- **-P:**将容器内部使用的网络端口随机映射到我们使用的主机上。

```
runoob@runoob:~#  docker ps
CONTAINER ID        IMAGE               COMMAND             ...        PORTS                 
d3d5e39ed9d3        training/webapp     "python app.py"     ...        0.0.0.0:32769->5000/tcp
```

Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 32769 上。

这时我们可以通过浏览器访问WEB应用

我们也可以通过 -p 参数来设置不一样的端口：

```
runoob@runoob:~$ docker run -d -p 5000:5000 training/webapp python app.py
```

##### 网络端口的快捷方式

通过 **docker ps** 命令可以查看到容器的端口映射，**docker** 还提供了另一个快捷方式 **docker port**，使用 **docker port** 可以查看指定 （ID 或者名字）容器的某个确定端口映射到宿主机的端口号。

上面我们创建的 web 应用容器 ID 为 **bf08b7f2cd89** 名字为 **wizardly_chandrasekhar**。

我可以使用 **docker port bf08b7f2cd89** 或 **docker port wizardly_chandrasekhar** 来查看容器端口的映射情况。

```
runoob@runoob:~$ docker port bf08b7f2cd89
5000/tcp -> 0.0.0.0:5000
runoob@runoob:~$ docker port wizardly_chandrasekhar
5000/tcp -> 0.0.0.0:5000
```

