### 一.docker简介

>Docker是一个开源的引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器。开发者在笔记本上编译测试通过的容器可以批量地在生产环境中部署，包括VMs（虚拟机）、bare metal、OpenStack 集群和其他的基础应用平台。
######  官网：https://www.docker.com/
######  仓库：https://hub.docker.com/
#####  IT 基础设施领域的发展] 
![image](https://raw.githubusercontent.com/liudaye008/dockerIntroduce/master/it.png)

*  第一张图显示的是一台物理机器或一台硬件服务器。通常，我们在构建应用程序时使用的是宿主操作系统提供的资源，在部署应用程序时也使用了相同的模式。但如果你想扩展应用程序该怎么办呢？在某些时候，你可能需要另一台硬件服务器。而随着数量不断增加，成本和其他资源（如硬件和能源消耗）也会随之增加。
*  第二张图中看到的，虚拟机具有自己的客户操作系统，运行在单个物理机或宿主操作系统中。我们因此能够运行多个应用程序，而无需安装大量物理机。宿主操作系统可以确保在不同虚拟机之间进行系统性的资源分配和负载均衡。
> 我们公司的内网环境，有150个左右的实例，而真正的物理机器不到10台。支持了：java/php/node/nginx/elk/redis等各个不同服务。可以说我们的运维还是很强大的。
*  虚拟机降低了软件维护的难度和成本，不过仍然可以进一步优化。例如，并非所有的应用程序在客户操作系统环境中都会按预期运行。此外，即使是运行简单的进程，客户操作系统也需要大量资源。  
这些问题促成了下一个创新：容器化。与特定于操作系统的虚拟机不同，容器特定于应用程序，因为更加轻量级。此外，虚拟机可以运行多个进程，而容器作为单个进程运行。于是：
1.  我们可以在物理机上运行多个容器，或者甚至可以考虑在单个虚拟机上运行容器。无论是哪种情况，它都可以解决应用程序相关的问题。
2.  容器化与虚拟化之间并不是竞争关系，而是一种互补，用以进一步优化 IT 软件基础设施。

#####  一句话概况
Docker 是全球领先的软件容器化平台，它将微服务封装进我们所说的 Docker 容器，然后进行独立的维护和部署。每个容器都将负责一个特定的业务功能。

#####  举例
例如，假设有三位开发人员正在开发此应用程序，他们每个人都有自己的开发环境。其中一个开发人员可能在他的机器上运行 Windows 操作系统，而第二个开发人员可能运行 Mac OS，第三个开发人员会更喜欢基于 Linux 的操作系统。他们每个人都需要花费数小时的时间将应用程序安装到各自的开发环境中，并且需要做额外的工作将它们部署到云端。这一过程并不那么顺畅，在将这些应用程序部署到云基础设施上时，他们之间总是会发生摩擦。  
借助 Docker，可以使应用程序独立于主机环境。因为采用了微服务架构，所以现在可以将每个服务封装到 Docker 容器中。Docker 容器是轻量级的，并且资源是隔离的，通过它可以构建、维护、发布和部署应用程序。

#####  优点
1.  Docker 是一款非常流行的软件，有强大的社区支持，并专门为微服务而构建。
2.  与虚拟机相比，它是轻量级的，在成本和资源消耗方面颇具优势。
3.  它为开发和生产环境提供了一致性，非常适合用于构建云原生应用程序。
4.  它为持续集成和部署提供了便利。
5.  Docker 可与 AWS、Microsoft Azure、Ansible、Kubernetes、Istio 这些流行的工具和服务集成。



### 二.docker实操  

1. 下载docker  

2. `docker pull rongxing/ubuntulnmp:1.3`

3. `docker run -d -p 80:80 -v ~/code:/var/www/html rongxing/ubuntulnmp:1.3 /sbin/init`  
    将本地的`~/code`目录挂在ubuntu的`/var/www/html`目录。
4. `docker ps` 

    `docker exec -it 进程id bash`  

    进入新镜像，启动nginx、mysql、php7.0-fpm服务  

    `service nginx start`   `service php7.0-fpm start`   `service mysql start`

5. 本地`hosts`文件，新增：`127.0.0.1 god.sc`  

6. 本地`~/code`目录放上调试的php代码  

###  三.制作自己的docker镜像
###### 1.下载ubuntu:16.04镜像
    docker pull ubuntu:16.04
###### 2.运行ubuntu镜像
    docker run -i -t ubuntu:16.04 bash
###### 3.在ubuntu镜像中搭建lnmp环境
    更新ubuntu系统 
    apt-get update
    
    安装php7.0  
    apt-get install  php
    
    安装nginx   
    apt-get install nginx
    
    安装mysql   
    apt-get install mysql5.6
    
    启动nginx、mysql、php7.0-fpm服务    
    service nginx start
    service mysql start
    service php7.0-fpm start
    
<pre><code>配置nginx:
index index.php index.html index.htm index.nginx-debian.html;
    location ~ \.php$ {
                    include snippets/fastcgi-php.conf;
            #
            #      fastcgi_pass 127.0.0.1:9000;
                    # With php7.0-fpm:
                    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
}</code></pre>
    退出镜像    
    [exit](https://note.youdao.com/)
    
###### 四、生成新的镜像
    查看之前编辑的镜像id    
    docker ps -l
    
    保存之前编辑的镜像到一个新镜像  
    docker commit -m "提交信息" --author "作者" 镜像id  新镜像名

###### 五、运行镜像
    docker images  
    
    选中IMAGE ID
    docker run -t -i （IMAGE ID）  /bin/bash

    docker ps -a  
    查看镜像id
    
    保存本地镜像：
    docker commit -m "提交信息" --author "作者" 镜像id  新镜像名
    
    本地保存好镜像后，去[https://hub.docker.com/](https://hub.docker.com/)建立远程仓库
    
    修改本地镜像名称与远程仓库一致：
    docker commit -m '修改本地镜像名称' -a '荣兴' 4c6b397d0c8f rongxing/ubuntulnmp:1.0

    推送镜像：
    docker push rongxing/ubuntulnmp:1.0
