# Docker

### 1.Docker简介

#### 1.1Docker是什么？

解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术

#### 1.2Docker的基本组成（三要素）

镜像（image）：镜像是一个只读的模板，可以用开创建Docker容器，一个镜像可以创建多个容器

容器（container）：容器是用镜像创建的运行实例，可以把容器看作是一个简易版的Linux环境和运行在其中的应用程序

仓库（repository）：仓库是集中存放镜像文件的场所

| Docker | 面向对象 |
| ------ | -------- |
| 镜像   | 类       |
| 容器   | 对象     |



### 2.Docker安装

#### 2.1安装前提

CentOS 7 (64-bit) : 内核版本3.10以上

CentOS 6.5(64-bit)或更高版本 : 内核版本为2.6.32-431或者更高

- 查看内核版本

```shell
uname -r
```

- 查看CentOS系統版本

```shell
cat /etc/redhat-release
```

#### 2.2安装(centos7 以上)

官网：<https://docs.docker.com/engine/install/centos/> 

- 安装步骤

1. 卸载旧版本

   ```shell
   $ sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

2. 安装`yum-utils`软件包（提供`yum-config-manager` 实用程序） 

   ```shell
   $ sudo yum install -y yum-utils
   ```

3. 设置稳定的存储库 

   ```
   $ sudo yum-config-manager \
       --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
   ```

4. 安装*最新版本*的Docker Engine和容器，或者转到下一步安装特定版本 

   ```shell
   $ sudo yum install docker-ce docker-ce-cli containerd.io
   ```

5. 启动Docker 

   ```shell
   $ sudo systemctl start docker
   ```

6. 通过运行`hello-world` 映像来验证是否正确安装了Docker Engine 

   ```shell
   $ sudo docker run hello-world
   ```

#### 2.3安装后的配置与检查

1. 配置镜像加速器

```shell
sudo mkdir -p /etc/docker 
sudo tee /etc/docker/daemon.json <<-'EOF' 
{
"registry-mirrors":["阿里云加速器地址"] 
} 
EOF 
sudo systemctl daemon-reload 
sudo systemctl restart docker 
```

2. 检查是否配置加速器成功

```shell
ps -ef | grep docker
```











































































