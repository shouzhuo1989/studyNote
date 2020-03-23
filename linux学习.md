- 用户

  ```
  配置文件位置：/etc/passwd
  口令配置文件（密码和登陆信息，是加密的）：/etc/shadow
  添加：useradd 用户名
  添加时指定组名：useradd -g 组名 用户名
  usermod -g police xh 》改变xh的所属组为police
  ```

- 组

  ```
  配置文件位置：/etc/group
  添加：groupadd 组名
  ```

  

- 运行级别

  ```
  配置文件：/etc/inittab
  ```


- 文件权限

  ```
  修改文件拥有者：chown   用户名 文件名
  修改文件所属组：chgrp   用户名 文件名
  修改文件权限：chmod u=rwx,g=rwx,o=rwx 文件名
  chown -R tom kk/  》将kk目录下的所有子目录和文件的所有者改为tom
  chgrp -R police kk/ 》将kk目录下的所有子目录和文件的所属组改为police 
  ```

  