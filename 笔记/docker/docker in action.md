1. 什么是docker
   1.  docker使用现有的容器引擎，根据最佳实践构建的解决方案，包括一个命令行程序 一个后台守护进程 一系列远程服务 
   2.  目的: 用于解决常见的软件问题 包括软件的安装 运行 发布 和删除软件 
   3.  实现原理: 通过unix容器实现的,任何在docker中运行的软件就是在容器中运行的
   4.  容器技术：jail/linix命名控件和cgroups
   5.  镜像：docker系统中组基本的交付单位，一个容器中运行程序的所有文件的捆绑快照，用来创建容器
   6.  特点： 
       1. 组织有序 依赖不同
       2. 可移植，程序和程序依赖一起移植
       3. 程序隔离
2. 使用docker
   1. docker run 
   2. docker stop
   3. docker start
   4. docker exec
   5. docker create
   6. docker ps
   7. docker top
   8. docker pull
   9. 注入环境变量
   10. 通过cat命令注入环境变量
   11. 使用shell脚本
3. 软件安装 镜像
   1. 本地查找->仓库查找->下载->构建容器->启动
   2. 软件查找 
      1. 仓库主机/用户名/容器短名:标签
   3. docker hub docker官方仓库
      1. 命令
         1. docker pull
         2. docker login
         3. docker logout
         4. docker search
      2. 直接发布镜像 
      3. 公开dockerfile，使用dockerhub的构建系统构建，可信赖的。
   4. 从注册服务器下载镜像
      1. docker pull 拉取
      2. docker rmi 删除
   5. 直接从镜像加载
      1. docker load -i
      2. docker save -o 从hub导出
   6. 项目分发 dockerfile
   7. union文件系统、MTC、chroot 适用与构建和分享镜像
4. 数据存储 存储卷 存储动态或专门的数据
   1. 存储卷 
      1. 允许容器和主机以及其他容器共享文件
      2. 把主机的目录挂载到容器中，网络等也可采用这种方式
      3. 绑定挂载卷 --v 本机目录:/容器目录:权限
         1. 多共享：主机依赖 
         2. 本质上是主机管理，docker不能删除
      4. 管理卷 --v 容器目录 映射的主机目录由docker管理，将容器数据和主机数据解耦
         1. 复制卷 将其他容器的存储卷复制过来,路径不能改变 --volumn_from
         2. 生命周期 和容器绑定，docker rm -v 指定删除管理卷，当然被其他容器引用的卷会被忽略
         3. 卷容器(共享出去的管理卷) 数据打包的卷容器（用于配置分发） 多态卷容器(基于不同配置打包)
5. 网络容器
   1. 容器原型
      1. closed容器：
         1. 不支持网络，只拥有一个本地回环接口，创建时没有创建网络接口
         2. --net none
         3. 当程序不需要外部网络时候使用
      2. bridge容器
         1. 最常用的容器，可以和外部网络通信
         2. --net bridge 
         3. --hostname 容器名称 
         4. --add—host 
         5. --dns --dns-search
         6. -p --expose 容器:本机 映射网络端口
         7. --ice false 禁止跨网络通信