# docker 记录


##  一. centos/fedora 安装 docker

### 1. 添加源一个就可以
- 中央仓库
    - `yum-config-manager --add-repo http://download.docker.com/linux/centos/docker-ce.repo`
- 阿里仓库 
    - `yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

### 2. 查看可以安装的版本
- `yum list docker-ce --showduplicates | sort -r`

### 3. 安装需要的版本
- `yum install docker-ce-18.03.1.ce`


## 二. systemd 添加自启动

### 使用非root帐号运行docker
- 添加docker用户组
    - `sudo groupadd docker`
    - yum安装的一般会提示组已经存在

- 将用户加入 docker group 内
    - `sudo gpasswd -a ${USER} docker`
    - ${USER} 是当前用户 也可以换成你想 运行 docker 命令的用户名
    
- 重启docker
    - `sudo systemctl restart docker`
    
- 切换当前会话到新 group 或者重启 X 会话
    - `newgrp - docker` 
    - 这一步是必须的，引起 groups 命令获取到的是缓存的组信息，刚添加的组信息未能生效
    

### 1. 启动dockerd
- 启动
    - `systemctl start docker`
- 添加开机启动
    - `systemctl enable docker`
    
### 2. run 一个helloworld
- `docker run hello-world`

### 3. 基本信息查看
- docker version # 查看docker的版本号，包括客户端、服务端、依赖的Go等
- docker info # 查看系统(docker)层面信息，包括管理的images, containers数等
- docker pull centos 下载centos镜像
- docker images [ centos ] 查看
- docker run -i -t centos /bin/bash


## 三. 一些基本使用

### 1. 基本镜像操作

#### 1.1 搜索镜像
- docker search <image> # 在docker index中搜索image
- `https://hub.docker.com/` 默认是这里的

#### 1.2 下载镜像
- docker pull <image>  # 从docker registry server 中下拉image

#### 1.3 查看/删除镜像 
- docker images： # 列出images
- docker images -a # 列出所有的images（包含历史）
- docker rmi  <image ID>： # 删除一个或多个image
 
 
## 四. 容器相关操作
 
### 1. 使用镜像创建容器
- docker run -i -t sauloal/ubuntu14.04
- docker run -i -t sauloal/ubuntu14.04 /bin/bash # 创建一个容器，让其中运行 bash 应用，退出后容器关闭

### 2. 查看容器
- 列出当前所有正在运行的container
    - `docker ps`
- 列出最近一次启动的container
    - `docker ps -l`
- 列出所有的container（包含历史，即运行过的container）
    - `docker ps -a`
- 列出最近一次运行的container ID
    - `docker ps -q`
    
### 3. 再次启动容器
- 开启/停止/重启container
    - docker start/stop/restart <container>

- 再次运行某个container （包括历史container）
    - docker start [container_id]

- 连接一个正在运行的container实例（即实例须为start状态，可以多个窗口同时attach 一个container实例）
    - docker attach [container_id]
    
- 启动一个container并进入交互模式（相当于先start，在attach）
    - docker start -i <container>

- 使用image创建container并进入交互模式, login shell是/bin/bash
    - docker run -i -t <image> /bin/bash
    
- 映射 HOST 端口到容器，方便外部访问容器内服务，host_port 可以省略，省略表示把 container_port 映射到一个动态端口。
    - docker run -i -t -p <host_port:contain_port> #：
    - 注：使用start是启动已经创建过得container，使用run则通过image开启一个新的container。
    
### 4. 删除容器

- 删除一个或多个container
    - docker rm <container...>
    
- 删除所有的container
    - docker rm `docker ps -a -q`
    - docker ps -a -q | xargs docker rm
 
 
### ５. 持久化容器与镜像

#### 5.1 通过容器生成新的镜像
- 将一个container固化为一个新的image，后面的repo:tag可选
    - `docker commit <container> [repo:tag]` 
    - 运行中的镜像称为容器
    - 你可以修改容器（比如删除一个文件），但这些修改不会影响到镜像
    - 不过，你使用docker commit 命令可以把一个正在运行的容器变成一个新的镜像
    
- 一个例子
    - `docker ps -a` 找打容器id 比喻我们 names 为 'ghostwwl_ubuntu1404' 的id 71d15fe6033f
        - 我们可能在这个容器里install了一堆 新的app
    - `docker commit 71d15fe6033f ghostwwl_newubuntu1404`
    - 你可以从 docker images 看到效果了
    
#### 5.2 容器持久化相关
- 持久化容器
    - docker export <CONTAINER ID> > /export/data/export.tar
- 导入持久化container
    - cat /export/data/export.tar | docker import - export:latest
    
#### 5.3 镜像持久化相关
- 持久化镜像
    - docker save <CONTAINER ID> > /export/data/save.tar
- 导入持久化image
    - docker load < /export/data/save.tar

#### 5.4 export/import与save/load的区别
- 导出后再导入(export/import)的镜像会丢失所有的历史，而保存后再加载（save/load）的镜像没有丢失历史和层(layer)。
- 这意味着使用导出后再导入的方式，你将无法回滚到之前的层(layer)，同时，使用保存后再加载的方式持久化整个镜像，就可以做到层回滚。
- （可以执行docker tag 来回滚之前的层）。

#### 5.5 一些其它命令
- 查看docker实例运行日志，确保正常运行
    - docker logs $CONTAINER_ID
    
- docker inspect <image|container> 查看image或container的底层信息
    - docker inspect $CONTAINER_ID
    - 查看容器ip 
        - `docker inspect $CONTAINER_ID|grep 'IPAddress'`
        - `docker inspect -f '{{ .NetworkSettings.IPAddress }}' $CONTAINER_ID`
    
- 寻找path路径下名为的Dockerfile的配置文件，使用此配置生成新的image
    - docker build <path> 
    
- 同上，可以指定repo和可选的tag
    - docker build -t repo[:tag] 
    
- 使用指定的dockerfile配置文件，docker以stdin方式获取内容，使用此配置生成新的image
    - docker build - < <dockerfile> 
    
- 查看本地哪个端口映射到container的指定端口，其实用docker ps 也可以看到
    - docker port <container> <container port> 


 部分资料参考 http://blog.csdn.net/fgf00/article/details/51893771


-------------------------

## 一些场景实例

### 容器与宿主机之间的数据共享
- 挂载本地目录到容器目录
    - docker run -it  --name ghostwwl_ubuntu1404 -v ~/data:/data -v ~/data1:/data1 sauloal/ubuntu14.04 /bin/bash 
        - 建立一个名为 ghostwwl_ubuntu1404 的交互式容器
        - `-v ~/data:/data` 挂载主机 `~/data` 目录到应容器目录 `/data` 目录
        - `-v ~/data1:/data1` 挂载主机 `~/data1` 目录到应容器目录 `/data1` 目录
    
- 查看已挂载目录
    - docker inspect ghostwwl_ubuntu1404

### 容器与容器之间的数据共享
- docker run -it --name ghostwwl_ubuntu1404_ --volumes-from ghostwwl_ubuntu1404 sauloal/ubuntu14.04 /bin/bash
    - 建立一个名为 ghostwwl_ubuntu1404_ 的交互式容器
    - 挂载容器名为 ghostwwl_ubuntu1404 的目录

- 这个时候最容易想到的是，我们可以通过多个容器 都挂载宿主机的同一个目录实现


### Docker部署私有仓库
- 首先需要有两台机器，一台作为仓库的Server，一台作为仓库的Client，用户通过Client来进行镜像的提交和拉取操作。
- 在Server端启动registry容器
    - `docker run -d --name registry -p 5000:5000 -v /opt/data/registry:/var/lib/registry --restart=always registry`
        - 建立了一个名为 registry 的守护式容器
        - 挂载了服务器目录 /opt/data/registry 
        - 映射端口为5000，这样在本地或者远程就可以通过该端口来访问仓库
- 在Client端进行Docker配置 [我们可以把这个当作镜像编译服务器 专门做 docker build 的]
    - 修改配置文件/etc/default/docker，在DOCKER_OPTS中加入"—insecure-registry 192.168.0.2:5000"。这样把一个通过认证的安全证书加到Registry服务中。
    - sudo systemctl restart docker
    

### 容器资源限制
- CPU限制
    - 例如: 最多使用2核
        - 默认的 CPU CFS「Completely Fair Scheduler」period 是 100ms。
        - 我们可以通过 --cpu-period 值限制容器的 CPU 使用。
        - 一般 --cpu-period 配合 --cpu-quota 一起使用。 
    - `docker run -it --rm --cpu-period=100000 --cpu-quota=200000 sauloal/ubuntu14.04 /bin/bash`

- 内存限制
    - 例如: 最大用100M
        - 禁用容器 swap 功能 `--memory-swappiness=0`
        - 限制swap空间 `--memory-swap 400M`
    - `ocker run -it --rm -m 100M --memory-swappiness=0 --memory-swap -1 sauloal/ubuntu14.04 /bin/bash`

- IO限制
    - 默认，所有的容器对于 IO 操作「block IO bandwidth – blkio」都拥有相同优先级。可以通过 `--blkio-weight` 修改容器 blkio 权重。
    - `--blkio-weight` 权重值在 10 ~ 1000 之间
    - 指定特定设备的权重
        - `--blkio-weight-device="DEVICE_NAME:WEIGHT"`
        -  可以指定某个设备的权重大小，如果同时指定 `--blkio-weight` 则以 `--blkio-weight` 为全局默认配置，针对指定设备以 `--blkio-weight-device` 指定设备值为主
        - `docker run -it --rm --blkio-weight-device "/dev/sda:100" sauloal/ubuntu14.04 /bin/bash`
    - 限制读写入速度
        - `--device-read-bps、--device-write-bps`
        - 限制写入速度1m `--device-write-bps /dev/sda:1mb`
        - 限制读出速度1m `--device-read-bps /dev/sda:1mb`

