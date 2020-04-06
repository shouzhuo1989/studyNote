mysql

- 安装

  ```
  docker run -d -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 --privileged=true --name mysql -v /home/kele/mysql/conf:/etc/mysql/conf.d -v /home/kele/mysql/logs:/logs -v /home/kele/mysql/data:/var/lib/mysql mysql:5.5
  
  -v /home/kele/mysql/conf:/etc/mysql/conf.d  》将主机/home/kele/mysql目录下的conf/my.cnf挂载到容器的/etc/mysql/conf.d
  -v /home/kele/mysql/logs:/logs  》将主机/home/kele/mysql/logs目录挂载到容器的/logs
  -v /home/kele/mysql/data:/var/lib/mysql 》将主机/home/kele/mysql/data目录挂载到容器的/var/lib/mysql
  -e MYSQL_ROOT_PASSWORD-123456 》初始化root用户的密码
  -d mysql:5.5 》后台运行没mysql:5.5
  
  docker exec -it 容器id /bin/bash
  
  链接：https://pan.baidu.com/s/1rSH78zfNeLdlMiXBUxkyuA
  提取码：vz68
  
  ```

- 逻辑架构

  ```
  1、连接层：包含本地socket通信和大多数基于客户端/服务端工具实现的类似与tcp/ip的通信。主要完成一些类似与连接处理、权限认证、及相关的安全方案。
  2、服务层：sql接口、缓存的查询、sql的分析和优化、内置函数的执行。在该层，服务器会解析查询并创建相应的解析树，并对其完成相应的优化，比如确定查询表的顺序，是否使用索引等;最后生成相应的执行操作。
  3、引擎层：负责数据的存储和提取，不同的存储引擎具有不同的适用场景;
  4、存储层：将数据存储与文件系统之中;
  ```

- 常用命令

  ```
  show engines; 》查看存储引擎
  ```

- MyISAM和InnoDB对比

  ![image-20200323104906779](/home/kele/.config/Typora/typora-user-images/image-20200323104906779.png)

  ![image-20200323105314878](/home/kele/.config/Typora/typora-user-images/image-20200323105314878.png)

- 执行计划

  ```
  1、explain是模拟mysql的查询优化器来分析我们的sql，然后给出执行计划;
  2、官网地址：http://dev.mysql.com/doc/refman/5.5/en/explain-output.html
  3、type字段的取值从最好到最差的排序：system>const>eq_ref>ref>range>index>all
  4、关联查询、BETWEEN、<、>、in、order by、group by都是可以用到索引的;
  5、索引的特点是有序，假如我们的查询条件可以使用到这种特性，那么就是可以使用索引的;
  ```

  - 问题

  ```
  1、索引默认是加载到内存中的，那么聚簇索引中是包含具体记录的，也会加载到内存中吗？
  2、如何导出表中的数据？
  ```
```
  
```

  ```
  
  
  ```