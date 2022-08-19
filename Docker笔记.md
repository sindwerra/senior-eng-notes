## Docker基础知识

### 重要命令行

1. docker image build (docker build) 基于当前路径下的DockerFile构建本地docker镜像
2. docker image push (docker push) 将当前镜像推向远端的docker-hub（就是一个类似于GitHub的公共仓库）
	- 一般命令是docker push <your_docker_hub_username>/<target_repo>:<your_custom_image_tag>
3. docker container run (docker run) 使用镜像run一个容器
	- 一般命令是docker run -d(后台运行) -it(进入容器且打开可交互终端) --name (指定容器名称)<custom_container_name> -p（容器与外部端口映射，左侧为外部端口，右侧为镜像中指定的容器端口） 8000:8080 <your_docker_hub_username>/<target_repo>:<your_custom_image_tag> 
4. docker attach <container_name | tag | id> 进入一个已经在运行的容器内部
5. docker run -d -p 80:80 -v /data:/usr/share/nginx/html nginx:latest （挂载卷的容器运行，左边是实际路径，右边是容器内路径）
6. docker volume inspect <volume_name | volume_id> （查看卷相关详细信息）
7. docker exec <container_name | container_id> <bash_command> （在运行中的指定容器内运行命令）
* * *

### 概念解析
1. 虚拟机VM和容器技术的不同点
	- VM技术是基于硬件构建的虚拟环境，而容器技术是基于操作系统内核构建的虚拟环境 
2. 什么是卷（volume）
	- 卷是一个指定的用来存放容器运行时产生数据的路径
	- 卷可以被多个容器共享 
3.  ENTRYPOINT和CMD的不同
	- ENTRYPOINT中的指令在容器运行时无法再进行修改，而CMD的指令在docker run中还可以通过参数修改
* * *

### 很重要的摘要
1. ![dfe27a59b47b4e34826a0374810efed3](/Users/duli/Desktop/typora-docs/Senior/dfe27a59b47b4e34826a0374810efed3.png)
2. 更加清晰的关于镜像与容器的解释 ![什么是镜像、什么是容器](/Users/duli/Desktop/typora-docs/Senior/什么是镜像、什么是容器.png)
3. 关于docker的网络![Docker的网络](/Users/duli/Desktop/typora-docs/Senior/Docker的网络.png)

***
### 引申出的问题

#### Docker的卷的原理以及使用方法

#### Docker的network的原理

![Docker的network原理](/Users/duli/Desktop/typora-docs/Senior/Docker的network原理.png)

#### DockerSecrets的使用方式
![DockerSecrets](/Users/duli/Desktop/typora-docs/Senior/DockerSecrets.png)

#### DockerCompose的内部网络分离方法
![DockerComposeNetwork1](/Users/duli/Desktop/typora-docs/Senior/DockerComposeNetwork1.png)
![DockerComposeNetwork2](/Users/duli/Desktop/typora-docs/Senior/DockerComposeNetwork2.png)

* * *
### 自我理解的一些结论
1. Docker-compose的作用：用来简化多个容器之间的网络通信
2. 接1的引申问题：那么docker本身有什么机制可以进行网络通信吗？答：有两种，第一种-legacy linking，即使用docker命令行的参数--link来连接容器，第二种-使用network，即使用docker创建一个叫network的对象，然后实现容器间的通信
3. 接2的引申问题：那么network有哪几种类型呢？答：[参见此](#docker的network的原理)


## Docker进阶知识

### 对于静态语言的服务构建
1. 使用builder-pattern
	1. 简单来说，可以分别使用两个dockerfile来跑服务，一个镜像只负责做构建工作，一个镜像只做运行工作![builderPattern](/Users/duli/Desktop/typora-docs/Senior/builderPattern.png)![builderPattern](/Users/duli/Desktop/typora-docs/Senior/builderPattern2.png)
	2. 为什么要这么做？主要原因是为了缩小镜像文件的大小（如上面这个例子，如果将运行镜像和构建镜像分开，运行镜像的大小可能只有混合镜像大小的百分之一）
2. multi-stage dockerfile
	1. 直接将操作分为多个步骤来构建镜像 
	![multiStageDockerfile](/Users/duli/Desktop/typora-docs/Senior/multiStageDockerfile.png)
	
### BuildKit
![buildKit](/Users/duli/Desktop/typora-docs/Senior/buildKit.png)
#### buildKit的配置方法
![buildKitConfig](/Users/duli/Desktop/typora-docs/Senior/buildKitConfig.png)

### Docker的命令底层原理
![DockerUndelyingMethodology](/Users/duli/Desktop/typora-docs/Senior/DockerUndelyingMethodology.png)
所以一个指令就会让镜像多一层，尽量减少层级就能缩小镜像的大小

### 改变dockerfile中的指令对镜像构造中缓存的影响
![ImapactOfChangeOfImageCode](/Users/duli/Desktop/typora-docs/Senior/ImapactOfChangeOfImageCode.png)

### 左右侧dockerfile代码的区别
![DockerCodeCompare](/Users/duli/Desktop/typora-docs/Senior/DockerCodeCompare.png)
左边的拷贝是全文件夹拷贝，这会导致只要这个host中这个路径的文件有任何改变，缓存就会失效，第三个命令RUN yarn install就会重新执行，然后安装所有的js包，而右边的代码可以解决此问题，即先将依赖配置文件拷贝过来，然后使用配置文件执行install，最后再将js源代码拷贝到镜像中，这样源代码的改变就不会影响到之前的依赖包的安装，只要依赖包的配置文件本身没有变化，RUN yarn install这个命令就会击中缓存

### dockerfile代码编写的一些原则
![PrinciplesOfWritingDockerCode](/Users/duli/Desktop/typora-docs/Senior/PrinciplesOfWritingDockerCode.png)
镜像的大小（也就是dockerfile文件层级的多寡)大部分时候和缓存命中的效率是成反比的
![ProfitOfMultiStageDockerFile](/Users/duli/Desktop/typora-docs/Senior/ProfitOfMultiStageDockerFile.png)

### dockerfile的配置编写方式
![HowtowriteDockerFiledConfig](/Users/duli/Desktop/typora-docs/Senior/HowtowriteDockerFiledConfig.png)

### docker的三种默认日志驱动
![DockerLogDefaultDriver](/Users/duli/Desktop/typora-docs/Senior/DockerLogDefaultDriver.png)