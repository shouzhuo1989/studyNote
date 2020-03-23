- 1、官网

  ```
  国外：http://www.docker.com
  中文：https://www.docker-cn.com/
  仓库：https://hub.docker.com
  ```

- 2、docker三大要素

  ```
  镜像（image）：就是一个只读的模板;可以用来创建docker容器;一个镜像可以创建多个容器;可以类比为java中的类;
  容器（container）：容器是用镜像创建的运行实例，可以被启动，开始，停止，删除;每个容器都是相互隔离的，保证安全的平台;
  仓库（repository）：是集中存放镜像的场所。仓库和仓库注册服务器是有区别的，后者上往往存放着多个仓库，每个仓库包含了多个镜像，每个镜像有不同的标签（tag）;仓库分为公开的和私有的;最大的公开仓库是docker hub，国内的公开仓库包括阿里云、网易云等;
  ```

- 3、安装

  ```
  lsb_release -a  获取系统版本   camel
  
  systemctl start docker  启动docker
  
  Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
  Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
  ```

- 4、常用命令

  ```
  docker version  
  docker info
  docker --help  查看帮助信息
  docker images  列出本地的镜像
  docker search -s 50  tomcat   列出点赞数大于50的tomcat
  docker pull tomcat  拉取tomcat镜像，默认是最新版
  ```

  - 容器常用命令

  ```
  创建和启动容器：docker [options] run [args]
  options常用选项：
  --name="新的容器名字" 》为容器指定一个名称
  -d 》后台运行容器，并返回容器id，即启动守护式容器 
  -i 》以交互式模式启动容器，通常-t同时使用
  -t 》为容器分配一个终端，通常与-i同时使用
  -P 》随即端口映射
  -p 》指定端口映射，有以下四种形式：
  	ip:hostPort:containerPort
  	ip::containerPort
  	hostPort:containerPort
  	containerPort
  查看docker上运行的容器：docker [options] ps [args]
  options常用选项：
  -a 》列出当前所有运行的容器和历史上运行过的
  -l 》显示最近创建的容器
  -n 》显示最近n个创建的容器
  -q 》只显示容器编号
  --no-trunc 》不截断输出
  退出容器：
  exit 》停止并退出
  ctrl+P+Q 》不停止退出
  启动容器：docker start
  重启容器：docker restart
  停止容器：docker stop / docker kill
  删除已经停止的容器： docker rm / docker -f rm   
  ```

  