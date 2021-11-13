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

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111112213660.png" alt="image-20211111112213660" style="zoom:30%;" />

**容器化之前是虚拟机，虚拟化有如下缺点：**

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

# 查看容器运行日志
docker logs 容器名/id

# 进入到某一容器的控制台
docker exec -it 容器名 /bin/bash

# 复制文件
docker cp 源路径 目标路径  #容器内的文件，可以用id指定：容器id:路径   
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

挂载：将容器内部的一个目录与大主机的一个目录建立起关系，使得在外部主机操作该目录的东西就像在容器内操作该目录的东西

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211107230403114.png" alt="image-20211107230403114" style="zoom:50%;" />

```shell
docker run \
--name=mynginx -d \
-p 88:80 \
-v data/html:/usr/share/nginx/html:ro nginx  #将主机的data/html目录挂载到容器的/usr/share/nginx/html，:ro表示只读，即容器内只可以读，只有外部主机可修改

# 挂载好之后，修改页面直接到主机的目录下修改就可以了，很nice
```

## 2.9 自己编写应用部署

**以前**

Java为例

- SpringBoot打包成可执行jar
- 把jar包上传给服务

- 服务器运行java -jar

**现在**

- 自己编写的应用打包成镜像，上传到docker hub，所有机器都安装Docker，都可以去docker hub拉镜像，所有机器都可以运行

- ```shell
  # 登录docker hub
  docker login
  
  #给旧镜像起名
  docker tag java-demo:v1.0  leifengyang/java-demo:v1.0
  
  # 推送到docker hub
  docker push leifengyang/java-demo:v1.0
  
  # 别的机器
  docker pull leifengyang/java-demo:v1.0
  
  # 别的机器运行
  docker run -d -p 8080:8080 --name myjava-app java-demo:v1.0 
  ```



# 3 Kubernetes

<img src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111112327732.png" alt="image-20211111112327732" style="zoom:50%;" />



随着容器化的到来，又带来了新的问题：部署了很多容器，怎么管理？由此引进了容器的编排管理工具—Kubernetes

Kubernetes：全称kubernetes，即k8s(k和s中间有8个字母)，为容器服务而生的一个可移植容器的编排管理工具

**kubernetes具有以下特性：**

- **服务发现和负载均衡**
  Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果进入容器的流量很大， Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。
- **存储编排**
  Kubernetes 允许你自动挂载你选择的存储系统，例如本地存储、公共云提供商等。

- **自动部署和回滚**
  你可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态 更改为期望状态。例如，你可以自动化 Kubernetes 来为你的部署创建新容器， 删除现有容器并将它们的所有资源用于新容器。
- **自动完成装箱计算**
  Kubernetes 允许你指定每个容器所需 CPU 和内存（RAM）。 当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源。

- **自我修复**
  Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的 运行状况检查的容器，并且在准备好服务之前不将其通告给客户端。
- **密钥与配置管理**
  Kubernetes 允许你存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。 你可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

Kubernetes提供了一个可弹性运行分布式系统的框架，可以满足你的扩展要求、故障转移、部署模式等。例如：k8s可以轻松管理系统的[Canary金丝雀部署（也称为灰度部署）](# 扩展：金丝雀/灰度/Canary部署)

## 3.1 K8s组织架构

Kubernetes Cluste(集群)r = N master node + N worker node  （N主节点 + N工作节点）

组件交互逻辑如下图：

![image-20211111153726490](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111153726490.png)

- 集群里面所有组件的交互都是通过==api-server==进行的
- 集群里面的网络访问都是通过==kube-proxy==进行的
- 集群里面运行的应用都得有个==容器运行时环境==
- 每一个机器节点都得有个监工（厂长）==kubelet==，去和api-server层汇报信息

- <img src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111155845788.png" alt="image-20211111155845788" style="zoom:40%;" />

## 3.2 K8s安装方式

先装个kubelet厂长，再装个kubectl(命令工具)，用kubeadm工具(集群工具)先init一个主节点，再用kubeadm join加入其他的节点

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111161236354.png" alt="image-20211111161236354" style="zoom:33%;" />



**安装kubeadm**

- 一台兼容的 Linux 主机。Kubernetes 项目为基于 Debian 和 Red Hat 的 Linux 发行版以及一些不提供包管理器的发行版提供通用的指令
- 每台机器 2 GB 或更多的 RAM （如果少于这个数字将会影响你应用的运行内存)

- 2 CPU 核或更多
- 集群中的所有机器的网络彼此均能相互连接(公网和内网都可以)

- - **设置防火墙放行规则**

- 节点之中不可以有重复的主机名、MAC 地址或 product_uuid。请参见[这里](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#verify-mac-address)了解更多详细信息。

- - **设置不同hostname**

- 开启机器上的某些端口。请参见[这里](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports) 了解更多详细信息。

- - **内网互信**

- 禁用交换分区。为了保证 kubelet 正常工作，你 **必须** 禁用交换分区。

- - **永久关闭**

**所有机器配置k8s官方要求的配置**

```bash
#各个机器设置自己的域名
hostnamectl set-hostname xxxx


# 将 SELinux 设置为 permissive 模式（相当于将其禁用）
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

#关闭swap
swapoff -a  
sed -ri 's/.*swap.*/#&/' /etc/fstab

#允许 iptables 检查桥接流量
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

# 刷新系统配置
sudo sysctl --system
```

**安装kubelet(厂长)、kubeadm(集群工具)、kubectl(命令工具)**

[ubuntu安装k8s](https://www.jianshu.com/p/f2d4dd4d1fb1)

kubelet 现在每隔几秒就会重启，因为它陷入了一个等待 kubeadm 指令的死循环

**使用kubeadm引导集群**

初始化master节点 -> 部署网络插件 -> master节点reday -> 工作节点join

**可以再部署一个可视化界面：dashboard**

## 3.3 k8s命令

> Docker把运行中的应用叫容器，k8s中叫pods 

```shell
#查看集群所有节点
kubectl get nodes

#根据配置文件，给集群创建资源
kubectl apply -f xxx.yaml

#查看集群部署了哪些应用
kubectl get pods -A 
```



# T41



























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

查看当前linux主机名字

```shell
hostname
```

设置linux主机名

```shell
hostnamectl set-hostname 新名字
```

刷新linux系统配置

```shell
sudo sysctl --system
```

检查应用状态

```shell
systemctl status 应用名
```

查看linux命令历史

```shell
history
```

定时查看状态

```shell
watch -n 1 ps -elf #在命令前加watch -n 1，1表示每隔一秒查看一次
```





# 扩展：金丝雀/灰度/Canary部署

金丝雀发布名称来源：

> 17世纪，英国矿井工人发现，金丝雀对瓦斯这种气体十分敏感。空气中哪怕有极其微量的瓦斯，金丝雀也会停止歌唱；而当瓦斯含量超过一定限度时，虽然人类毫无察觉，金丝雀却早已毒发身亡。当时在采矿设备相对简陋的条件下，工人们每次下井都会带上一只金丝雀作为“瓦斯检测指标”，以便在危险状况下紧急撤离。

- 金丝雀（Canary）发布也是一种发布策略，即灰度发布。
- 金丝雀发布指的是在生产环境中分阶段逐步更新后端应用的版本（需要具备流量控制能力），==在小范围验证符合预期之后，再推广至整个生产环境==。
- 金丝雀发布的好处：可以用真实环境测试新版本，当新版本存在问题时最多只影响部分用户，且支持安全快速的回滚策略（将路由到新版本上的流量切换到其他老版本机器上即可）。
- 蓝绿部署是准备两套系统，在两套系统之间进行切换；金丝雀策略是只有一套系统，逐渐替换这套系统
