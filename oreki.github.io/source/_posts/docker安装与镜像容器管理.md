---
title: docker安装卸载与镜像容器管理
toc: true
mathjx: true
cover: /2020/01/01/docker安装与镜像容器管理/head.png
tags:
  - Docker
categories:
  - 运维
  - Docker
abbrlink: 36368
date: 2020-01-01 22:26:42
update:
---
# Docker来来回回整都还没整清楚，先把安装卸载，添加和删除镜像整明白，方便重来。

### 安装与卸载Docker
1. 首先，更新软件包索引，并且安装必要的依赖软件，来添加一个新的 HTTPS 软件源：
~~~
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
~~~

2. 使用下面的 curl 导入源仓库的 GPG key：
~~~
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
~~~

3. 将 Docker APT 软件源添加到你的系统：
~~~
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
~~~
现在，Docker 软件源被启用了，你可以安装软件源中任何可用的 Docker 版本。

4. 运行下面的命令来安装 Docker 最新版本
~~~
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
~~~

5. 安装指定版本的Docker，首先列出 Docker 软件源中所有可用的版本
  ~~~
  sudo apt update
  apt list -a docker-ce
  ~~~
  可用的 Docker 版本将会在第二列显示。
  >docker-ce/focal 5:19.03.9~3-0~ubuntu-focal amd64

  通过在软件包名后面添加版本=<VERSION>来安装指定版本
  ~~~
  sudo apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
  ~~~

6. 安装完成后，Docker 服务将会自动启动。输入下面的命令来验证
~~~
sudo systemctl status docker
~~~

7. 验证安装过程
想要验证 Docker 是否已经成功被安装，你可以执行docker命令，前面不需要加sudo, 会自动运行一个测试容器
~~~
docker container run hello-world
~~~

8. 卸载Docker
  在卸载 Docker 之前，你最好移除所有的容器，镜像，卷和网络。
  运行下面的命令停止所有正在运行的容器，并且移除所有的 docker 对象
  ~~~
  docker container stop $(docker container ls -aq)
  docker system prune -a --volumes
  ~~~

  接下来你可以使用apt命令来卸载 Docke
  ~~~
  sudo apt purge docker-ce
  sudo apt autoremove
  ~~~

### Docker删除容器和镜像

>docker ps -aq # 列出所有容器ID
docker ps -a # 查看所有运行或者不运行容器
docker stop &#36;;(docker ps -a -q) # 停止所有的container（容器），这样才能够删除其中的images
docker rm &#36;(docker ps -a -q) # 如果想要删除所有container（容器）的话再加一个指令
docker container prune # 删除所有停止的容器
docker stop Name或者ID  # 停止一个容器
docker start Name或者ID  # 启动一个容器
docker kill Name或者ID  # 杀死一个容器
docker restart name或者ID # 重启一个容器



>docker images # 查看当前有些什么images
docker rmi imageid # 删除images（镜像），通过image的id来指定删除谁
docker rmi &#36;(docker images | grep "^<none>" | awk "{print &#36;3}") # 想要删除untagged images，也就是那些id为的image的话可以用
docker rmi &#36;(docker images -q) # 要删除全部image（镜像）的话
docker rmi -f &#36;(docker images -q) # 强制删除全部image的话
