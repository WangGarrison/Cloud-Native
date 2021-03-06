# 零：云原生简介

云 + 原生：原生应用（c++、java开发的应用）上云的过程，以及云上的一系列解决方案

![image-20211102112209639](img/%E4%BA%91%E5%8E%9F%E7%94%9F.img/image-20211102112209639.png)

# 一：云平台

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

# 二：容器化

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



# 三：Kubernetes

<img src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211111112327732.png" alt="image-20211111112327732" style="zoom:50%;" />



随着容器化的到来，又带来了新的问题：部署了很多容器，怎么管理？由此引进了==容器的编排管理工具—Kubernetes==

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

## 3.4 NameSpace

名称空间，用来对集群资源进行隔离划分。默认只隔离资源，不隔离网络

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211116150427422.png" alt="image-20211116150427422" style="zoom:30%;" />

```shell
#创建命名空间
kubectl create ns helllo

#删除命名空间
kubectl delete ns hello
```

使用配置文件也可以创建namespace：

新建一个yaml文件

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: hello
```

使用命令 `kubectl apply -f hello.yaml` 执行配置文件的内容，hello命名空间就创建好了；要删除的话，执行命令：`kubectl delete -f hello.yaml`

## 3.5 Pod容器组

pod是k8s管理的最小单位，pod里面可运行容器（pod像一个宿舍，里面住着一些打工仔(容器))

一个pod里面可以运单个容器或多个容器，建议是单个容器

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211116152652875.png" alt="image-20211116152652875" style="zoom:33%;" />

```shell
kubectl run mynginx --image=nginx  #启动一个pod，里面是nginx容器

kubectl get pod  #查看默认命名空间的pod，等价于 kubectl get pod -n default

kubectl get pod -A #查看所有命名空间的pod

kubectl describe pod pod名字   #查看指定pod的详细描述信息

# 每个pod，k8s都会给分配一个ip，集群中的任意一个机器都能通过Pod分配的ip来访问这个pod
kubectl get pod -o wide  #查看pod运行时ip与状态，-owide可以连着写

curl 192.168.169.136 #访问ip的默认端口

kubectl logs pod名  #查看pod运行日志

kubectl logs -f pod名  #阻塞式追踪pod日志，即当前shell阻塞，有新日志就会继续显示出来

kubectl delete pod mynginx #删除默认名称空间的pod，-n default

#进入到pod内部的bash控制台
kubectl exec -it mynginx -- /bin/bash
```

使用配置文件创建pod：

`vi pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata: 
  labels:
    run: mynginx
  name: mynginx
  namespace: default
spec:
  containers:
  - image: nginx
    name: mynginx
```

`kubectl apply -f pod.yaml`

通过配置文件删除pod：`kubectl delete -f pod.yaml`

也可在可视化界面通过点击来创建pod

**pod里面运行两个容器示例：（一个宿舍里面住了两个打工仔）**

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: myapp
  name: myapp
spec:
  containers:
  - image: nginx
    name: nginx
  - image: tomcat:8.5.68
    name: tomcat
```

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211121120358256.png" alt="image-20211121120358256" style="zoom:50%;" />

通过ip+端口号可以访问到不同的容器：nginx运行在80端口，tomcat运行在8080端口

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211121121119343.png" alt="image-20211121121119343" style="zoom:40%;" />

注意：两个应用不能共用一个端口，这样操作会让一个启动失败

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211121123746861.png" alt="image-20211121123746861" style="zoom:50%;" />

## 3.6 Deployment（工作负载）

控制Pod，使Pod拥有==多副本，自愈、扩缩容能力==

自愈能力：使用deployment部署的应用，使用kubectl delete删除应用，会自动重启该应用，要真删得删deployment

```shell
# 清除所有Pod，比较下面两个命令有何不同效果？
kubectl run mynginx --image=nginx

kubectl create deployment mytomcat --image=tomcat:8.5.68

# 自愈能力: 使用deployment部署的应用，使用kubectl delete删除应用，会自动重启该应用，要真删得删deployment
```

**Deployment的多副本功能**

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211121133128829.png" alt="image-20211121133128829" style="zoom:40%;" />

```shell
kubectl create deployment my-dep --image=nginx --replicas=3
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: my-dep
  name: my-dep
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-dep
  template:
    metadata:
      labels:
        app: my-dep
    spec:
      containers:
      - image: nginx
        name: nginx
```

**Deployment扩缩容功能**

```shell
kubectl scale deploy/my-dep --replicas=5  #由1个扩容到5个
kubectl scale deploy/my-dep --replicas=2  #由5个缩容到2个

#这就很像一些电商节，流量高峰期需要扩容，过后需要缩容
```

![image-20211121133439767](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211121133439767.png)

**Deployment的自愈&故障转移**

自愈：异常下线，自动重启

故障转移：一个结点宕机，将坏结点的pod转移到别的结点上。（故障pod的信息会实时通过APIServer存储到etcd里面，如果宕机了，会重新读取etcd里面的内容来达到故障转移）

**滚动更新**

![image-20211204095609875](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211204095609875.png)

先启动一个v2版本的绿色Pod，等它运行成功了，把v1版本的Pod替换掉，全程不需要停机（启动一个新的后，再杀死一个老的，这样一个个挨个去升级，这就称为滚动更新）

```shell
kubectl set image deployment/my-dep nginx=nginx:1.16.1 --record
kubectl rollout status deployment/my-dep

# 修改
kubectl edit deployment/my-dep
```

**版本回退**

```SHELL
#历史记录
kubectl rollout history deployment/my-dep

#查看某个历史详情
kubectl rollout history deployment/my-dep --revision=2

#回滚（回到上次）
kubectl rollout undo deployment/my-dep

#回滚（回到指定版本）
kubectl rollout undo deployment/my-dep --to-revision=2
```

版本回退也是一个滚动更新的过程

> 除了Deployment，k8s还有StatefulSet、DaemonSet、Job等类型资源。都称为工作负载，有状态应使用StatefulSet部署，无状态应使用Deployment部署
>
> 在k8s中，不直接布置pod，虽然说pod才是应用的载体，但是一般都是使用如下图这些工作负载来控制pod的
>
> ![image-20211204102113141](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211204102113141.png)

## 3.7 Service（服务发现）

==Service：将一组pods公开为网络服务的抽象方法==，具有服务发现和负载均衡的功能

Service的ClusterIP模式如下图，在集群内可以通过Service暴露的ip/域名来访问：

![image-20211204112601412](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211204112601412.png)

```shell
kubectl expose deploy my-dep --port=8000 --target-port=80  --type=ClusterIP
```

**Service-NodePort模式**（集群外也可以访问）

```shell
kubectl expose deploy my-dep --port=8000 --target-port=80  --type=NodePort
```

![image-20211204113154839](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211204113154839.png)

## 3.8 Ingress统一网关入口

> service是为一组pod服务提供一个统一集群内访问入口或外部访问的随机端口，而ingress做的是通过反射的形式对请求分发到对应的service上（==service是pod的入口，ingress是service的上层入口==）
>
> 引入ingress的原因：service是很多的，入口不统一的话不方便管理

Ingress：service的统一网关入口

![image-20211204115445393](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211204115445393.png)

> Ingress有一些高级用法：路径重写、流量限制等

## 3.9存储抽象

把挂载的统一抽象到存储层，

![image-20211205121540502](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211205121540502.png)

PV：Persistent Volume持久卷（真正存数据的），将应用需要的持久化的数据保存到指定的地方

PVC：Persistent Volume Claim 持久卷声明（场地声明书），声明需要使用的持久卷的规格

![image-20211206105447344](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211206105447344.png)

挂载目录用PV、PVC

挂载配置文件用Config Map

## 3.10 Secret

Secret对象类型用来保存敏感信息，例如密码、OAuth令牌和SSH密钥。将这些信息放在Secret中更安全



# 四：KubeSphere

KubeSphere：是==基于 Kubernetes== 构建的分布式、多租户、多集群、企业级==开源容器平台==，具有强大且完善的网络与存储能力，并通过极简的人机交互提供完善的多集群管理、CI / CD 、微服务治理、应用管理等功能，帮助企业在云、虚拟化及物理机等异构基础设施上快速构建、部署及运维容器架构，实现应用的敏捷开发与全生命周期管理。



## 4.1 Linux单节点部署KubeSphere

1、开通服务器，服务器防火墙放行30000~32767

![image-20211207104436651](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211207104436651.png)

2、[在 Linux 上以 All-in-One 模式安装 KubeSphere](https://kubesphere.com.cn/docs/quick-start/all-in-one-on-linux/)



## 4.2 多租户系统实战

![img](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/1631589337850-64ced113-11ed-4c25-99b0-5b101995cecf.png)

## 4.3 部署应用

应用部署三要素：

- 应用的部署方式：选一种工作负载
- 应用的数据挂载：Pod数据挂载到哪里、配置挂载到哪里
- 应用的可访问性：部署的应用网络应该如何访问，即服务

![image-20211210115117665](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211210115117665.png)

MySQL部署分析示意图：

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211210121841440.png" alt="image-20211210121841440" style="zoom:50%;" />

## 4.4 RuoYI-Cloud部署实战

![image-20211227100311714](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211227100311714.png)

1. 执行sql，准备数据库，配置redis地址

2. 照着文档，本地启动各个微服务

3. 云上部署

   ![image-20211228111941377](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20211228111941377.png)

**健康检查机制：**k8s不断地给pod发送请求去检查这个pod是否在正常工作，如果这个pod异常，k8s重启这个pod

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220119235523223.png" alt="image-20220119235523223" style="zoom:40%;" />

# 五：DevOps

## 5.1 DevOps简介

==DevOps：自动化的部署流程，Develop and Operation 开发与运维自动化配合，实现持续集成和持续交付==

==DevOps **是一系列做法和工具**，可以使 IT 和软件开发团队之间的**流程实现自动化**。==其中，随着敏捷软件开发日趋流行，**持续集成 (CI)** 和**持续交付 (CD)** 已经成为该领域一个理想的解决方案。在 CI/CD 工作流中，每次集成都通过自动化构建来验证，包括编码、发布和测试，从而帮助开发者提前发现集成错误，团队也可以快速、安全、可靠地将内部软件交付到生产环境。

![image-20220120102013551](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220120102013551.png)

![image-20220120102445107](img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220120102445107.png)

<font color='blue'>**简而言之，只要代码发生了变更，通过一系列的自动化流程，就直接能看到云平台代码变更后的效果，开发人员与运维人员之间的协同就不需要做太多工作了**</font>

## 5.2 项目部署实例

尚医通项目架构图

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220120105004668.png" alt="image-20220120105004668" style="zoom:50%;" />

<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220120105239374.png" alt="image-20220120105239374" style="zoom:50%;" />

 **部署过程**

- 先部署中间件：==MySQL、MongoDB、RabbitMQ==等（运维人员部署），将代码中的初始化SQL文件在数据库中执行好
- 生产与开发配置隔离
- 再部署各个微服务（通过流水线部署）：拉取代码-》项目编译-》并发构建各个微服务镜像-》将镜像推送到镜像仓库

​	<img align='left' src="img/%E4%BA%91%E5%8E%9F%E7%94%9F%E3%80%81%E5%AE%B9%E5%99%A8%E3%80%81k8s.img/image-20220122172842339.png" alt="image-20220122172842339" style="zoom:40%;" />

























## 5.3 webhook（钩子）

1. 每个项目都有流水线文件
2. 每次修改完项目，手动点击运行流水线
3. 希望：每次修改完项目，代码推送后，流水线自动运行
   - ==写代码并提交---》gitee---》给指定的地方发请求(webhook)---》kubesphere平台感知到---》启动流水线==
   - 注意：流水线监控的应该是发版分支，平时开发的推送不应该跑到流水线上

> 尚硅谷云原生第一季前130p完毕





















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

# 扩展：Yaml

Yaml：**YAML**（/ˈjæməl/，尾音类似*camel*骆驼）是一个可读性高，用来表达资料序列化序列化)的格式。多用于配置文件

*YAML*是"YAML Ain't a Markup Language"（YAML不是一种标记语言标记语言)）的递归缩写。在开发的这种语言时，*YAML* 的意思其实是：=="Yet Another Markup Language"（仍是一种标记语言）==，但为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名。
