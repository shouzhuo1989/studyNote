mysql

```
docker run -p 3306:3306 --name mysql -v /home/kele/mysql/conf:/etc/mysql/conf.d -v /home/kele/mysql/logs:/logs -v /home/kele/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD-123456 -d mysql:5.5


-v /home/kele/mysql/conf:/etc/mysql/conf.d  》将主机/home/kele/mysql目录下的conf/my.cnf挂载到容器的/etc/mysql/conf.d
-v /home/kele/mysql/logs:/logs  》将主机/home/kele/mysql/logs目录挂载到容器的/logs
-v /home/kele/mysql/data:/var/lib/mysql 》将主机/home/kele/mysql/data目录挂载到容器的/var/lib/mysql
-e MYSQL_ROOT_PASSWORD-123456 》初始化root用户的密码
-d mysql:5.5 》后台运行没mysql:5.5

docker exec -it 容器id /bin/bash

docker run -d -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 --privileged=true --name mysql mysql:5.5

```

