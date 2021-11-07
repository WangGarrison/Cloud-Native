# 0 云原生简介

云 + 原生：原生应用（c++、java开发的应用）上云的过程，以及云上的一系列解决方案

![image-20211102112209639](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211102112209639.png)

# 1 云平台

云与本地相对，云平台好处：

- 环境统一
- 按需付费
- 即开即用
- 按需付费

国内常见云平台：阿里云、百度云、腾讯云、华为云、青云

国外常见云平台：亚马逊AWS、微软Azure

> 部署云计算资源有三种不同的方法：公有云、私有云和混合云。采用的部署方法取决于业务需求

## 1.1 公有云

即购买云服务商提供的公共服务

优势：

- 成本低：无需购买硬件或软件，仅对使用的服务付费
- 无需维护：维护由服务提供商提供
- 近乎无限制的缩放性：提供按需资源，可满足业务需求
- 高可靠性：具备众多服务器，确保免受故障影响
  - 可用性：N个9，比如说：可靠性为99.999%，则一年的故障时间为：365\*24*3600\*(1-99.999%)

缺点：

- 将数据托管给云厂商，数据可能有泄漏风险

## 1.2 私有云

自己搭建云平台，或者购买，服务和基础架构始终在私有网络上进行，硬件和软件专供内部使用

优势：

- 控制力强，隐私级别高：所有数据由内部维护，有高控制力，以及更高的隐私级别
- 灵活性强：组织可以自定义云环境以满足特定业务需求

## 1.3 安全组

云服务器的防火墙，控制哪些端口可以供外部访问，哪些外部不可以访问

## 1.4 公有ip、私有ip

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211102123227766.png" alt="image-20211102123227766" style="zoom:50%;" />

公有ip：在任何地方都可以访问到该ip

 私有ip：内部网卡真正用的ip

在百度直接搜索IP会显示公网IP，在cmd中输入ipconfig，或者在Linux终端下输入ifconfig，会显示出私有ip

公网IP一般是运营商分配的，公网ip才能上网，但是不可能给每一个电脑分配一个IP，ipv4肯定是不够的。所以需要私有IP，这种ip一般是用于局域网的管理，不能直接连上互联网，必须通过公网ip上网。

在很早的时候就预料到了ipv4可能不足，所以在每一类的ip地址中都预留了一部分地址作为私有ip类型 ip范围 私有地址范围

- A 1.0.0.0~126.255.255.255 10.0.0.0~10.255.255.255
- B 128.0.0.0~191.255.255.255 172.16.0.0~172.31.255.255
- C 192.0.0.0~223.255.255.255 192.168.0.0~192.168.255.255

这也是为什么大多数时候，你使用ipconfig查到的一般就只是以172.开头的b类私有Ip，或者以192.168开头的c类私有Ip.简单的说，s私有ip有底下的几个限制：

- 私有 IP 的路由信息不能对外散播 (只能存在内部网络)；
- 使用私有 IP 作为来源或目的地址的封包，不能透过 Internet 来转送 (不然网络会混乱)；
- 关于私有 IP 的参考纪录(如 DNS)，只能限于内部网络使用 (一样的原理啦)

**局域网集群环境下，服务器之间使用私有ip较好，这样就不会走公有ip的付费，速度也要快些（公网ip会有带宽的限制，比如买的是1M的带宽，私有ip就直接走内网，不受带宽限制**

<img src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211102123636604.png" alt="image-20211102123636604" style="zoom:50%;" />

## 1.5 VPC、划分网段

==vpc==：Virtual Private Cloud，私有网络，是公有云上自定义的逻辑隔离网络空间，是一块可我们自定义的网络空间

<img src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211102125643795.png" alt="image-20211102125643795" style="zoom:50%;" />

# 2 容器化

## 2.1 Docker基本概念

Docker解决的问题：（所有的软件打包成统一的镜像，便于分享和运行，这些镜像在docker hub里面去找）

- 应用构建：Docker把软件构建统一了下，Java、C++、JavaScript编好的程序通过docker build统一打成软件包，该包 称之为镜像

- 应用分享：所有软件的镜像放到一个指定地方—docker hub，类似于安卓手机里面的应用市场
- 应用运行：程序都统一打包成镜像，通过docker run直接运行

Docker基本术语：

- Docker_Host：安装Docker的主机
- Docker Daemon：运行在Docker主机上的Docker后台进程
- Client：操作Docker主机的客户端、命令行
- Registry：镜像仓库，Docker Hub
- Images：镜像、带环境打包好的程序，可以直接启动运行
- Continers：容器，由镜像启动起来正在运行中的程序
- 镜像与容器关系：==镜像是容器运行的基础，容器是镜像运行后的形态==

基本使用流程：

- 装好Docker客户端，然后去软件市场寻找镜像，下载并运行，查看容器状态日志等排错
- <img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211104164014126.png" alt="image-20211104164014126" style="zoom:40%;" />

## 2.2 容器化时代

容器化之前是虚拟机，虚拟化有如下缺点：

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211104161340644.png" alt="image-20211104161340644" style="zoom:40%;" />

而容器化有如下优点：

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211104161445142.png" alt="image-20211104161445142" style="zoom:40%;" />



## 2.3 docker命令

![image-20211107180424107](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211107180424107.png)

```shell
# docker 启动
systemctl enable docker --now 

# 查看docker版本信息
docker info

# 下载镜像
docker pull 镜像名 #镜像名=镜像名:最新版本号，即默认下载最新版

# 查看已经下载的镜像
docker images 

# 删除某个镜像
docker rmi 镜像名
```

容器（运行中的程序）相关命令：

```shell
# 启动容器
docker run 设置项 镜像名
docker run --name=mynginx -d nginx  #-d表示后台运行
docker run --name=mynginx -d  --restart=always nginx  #--restart=always表示开机自动启动

# docker 查看运行中的程序
docker ps
docker ps -a #查看所有容器，包括停止运行的

# 停止正在运行的容器
docker stop 容器名/id

# 启动停止运行的容器
docker start 容器名/id

# 删除已经停止运行的程序
docker rm 容器名
docker rm -f 容器名 #强制删除正在运行中的程序

# 更新某一程序的设置项参数
docker update 容器名/id 设置项
docker update mynginx --restart=always
```

**docker run 与docker start区别**

docker run 只在第一次运行时使用，将镜像放到容器中，以后再次启动这个容器时，只需要使用命令docker start即可。docker run相当于执行了两步操作：将镜像放入容器中（docker create）,然后将容器启动，使之变成运行时容器（docker start）。

而docker start的作用是，重新启动已存在的镜像。也就是说，如果使用这个命令，我们必须事先知道这个容器的ID，或者这个容器的名字，我们可以使用docker ps找到这个容器的信息。


## 2.4 docker端口映射

通过公网ip访问到docker的程序，在参数项中设置 `-p 大主机端口:容器内程序运行的端口`

```shell
docker run --name=mynginx -d  --restart=always -p 88:80 nginx
```

![image-20211107184444176](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211107184444176.png)

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211107183745349.png" alt="image-20211107183745349" style="zoom:50%;" />

这样，就可以通过 `ip地址:大主机端口` 来访问docker程序

## 2.5 修改容器程序内容

> 示例：修改容器nginx的index文件，使得通过 `82.156.28.34:88`可以访问到自定义的html

先做端口映射，然后腾讯云服务器安全组打开88端口（即防火墙开放该端口）

```shell
docker run --name=mynginx -d  --restart=always -p 88:80 nginx
```

==进入容器nginx的bash控制台==

```shell
docker exec -it 容器id /bin/bash  #72f5bbf6ed11是nginx的程序id，bash可以换成sh
```

进入到容器的index目录，修改html文件

```shell
cd /usr/share/nginx/html
echo "<h1>Wang Test Docker<h1>" > index.html  #将双引号中内容写入到index.html中
```

浏览器网址输入：`82.156.28.34:88`，成功访问并显示

![image-20211107204245437](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211107204245437.png)

## 2.6 提交docker修改的内容

如果我按2.5那样把docker程序内容修改了，那如果该程序停止运行了并且镜像被删了，怎么能继续保留里面的东西呢，这就需要先提交一下修改的内容，落盘生成新的镜像，这样下次就可以启动新镜像

```shell
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
docker commit -a "wangjiaxin" -m "update了首页" 容器id wangnginx:v1.0 
```

下次就可以使用`docker run -d -p 88:80 wangnginx:v1.0`启动新镜像，就能再次看到自定义修改的内容了

## 2.7 其他机器使用该容器的办法

**方法一：通过本地保存，传输tar包实现**

先将容器保存为实体tar包

```shell
docker save [OPTIONS] IMAGE [IMAGE...]
docker save -o abc.tar wangnginx:v1.0 # 将新镜像保存为abc.tar
```

将该tar包可以通过scp命令传输给其他机器，然后其他机器通过docker load加载tar包，然后docker run启动该容器

```shell
docker load -i abc.tar
```

**方法二：通过远程仓库分享**

将镜像推送到docker hub，别人要用就去docker hub去下载

先去 [dockerhub](https://hub.docker.com/) 建立一个仓库，然后通过如下步骤推送

1、先给镜像打标签，即把镜像名字改成docker远程仓库的名字

```shell
docker tag local-image:tagname 仓库名:tagname  #给镜像打标签
docker tag wangnginx:v1.0 15691177603/wangnginx:v1.0
```

2、终端登录docker账户

```shell
docker login

# 退出登录 docker logout
```

3、推送

```shell
docker push 仓库名:tagname
docker push 15691177603/wangnginx:v1.0
```

![image-20211107215119516](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211107215119516.png)

## 2.8 挂载

## T17



# 附录：术语

==Paas==：Platform as a Service，指平台即服务， 把服务器平台作为一种服务提供的商业模式。云计算时代相应的服务器平台或者开发环境作为服务进行提供就成为了PaaS

==Saas==：Software as a Service，软件即服务，通过网络进行程序提供的服务称之为SaaS

==k8s==：全称kubernetes，为容器服务而生的一个可移植容器的编排管理工具

==KubeSphere==：KubeSphere 是在目前主流容器调度平台 Kubernetes 之上构建的企业级分布式多租户容器平台，提供简单易用的操作界面以及向导式操作方式，在降低用户使用容器调度平台学习成本的同时，极大减轻开发、测试、运维的日常工作的复杂度，旨在解决 Kubernetes 本身存在的存储、网络、安全和易用性等痛点。除此之外，平台已经整合并优化了多个适用于容器场景的功能模块，以完整的解决方案帮助企业轻松应对敏捷开发与自动化运维、微服务治理、多租户管理、工作负载和集群管理、服务与网络管理、应用编排与管理、镜像仓库管理和存储管理等业务场景。

==DevOps==：Development和Operations的组合词，是一组过程、方法与系统的统称，用于促进开发、技术运营和质量保障（QA）部门之间的沟通、协作与整合

==ECS==：Elastic(弹性) Compute Service，即弹性云服务器，是阿里云提供的性能卓越、稳定可靠、弹性扩展IaaS（Infrastructure（基础设施） as a Service）级别云计算服务

==vpc==：Virtual Private Cloud，私有网络，是公有云上自定义的逻辑隔离网络空间，是一块可我们自定义的网络空间

# 附录：Linux命令

查看指定路径目录结构

```shell
ls 路径
ls /  # /表示根目录，显示根目录结构
```

查看软件安装目录

```shell
whereis 软件名
wehreis nginx
```

重启应用程序

```shell
systemctl restart 程序名
systemctl restart nginx 
```

查看私有ip

```shell
ip a
ipconfig
```

查看linux内核版本

```shell
uname -r
```

linux重启

```shell
reboot
```

 linux文件传输

```shell
scp 要传输的文件 目标机器路径  #目标机器 用户名@ip:路径
```

