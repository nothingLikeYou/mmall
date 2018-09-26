# 阿里云(centos7)安装 mysql5.7

> author:zhangshaolin

> date:2018/5/31

- 下载 Mysql yum 包
 
  `wget http://repo.mysql.com/mysql57-community-release-el7-10.noarch.rpm`
  

- 安装软件源
  
  `rpm -Uvh mysql57-community-release-el7-10.noarch.rpm`
  
- 安装 mysql 服务端
  
  `yum install  -y  mysql-community-server`
  
- 启动 mysql

  ```$xslt
  service mysqld start
  systemctl start mysqld.service
  ```

- 修改临时密码

  > 为了加强安全性，MySQL5.7为root用户随机生成了一个密码，在error log中，关于error log的位置，如果安装的是RPM包，则默认是/var/log/mysqld.log。 
    只有启动过一次mysql才可以查看临时密码
    
- 查看mysql root用户默认密码
 
  `grep 'temporary password' /var/log/mysqld.log`
  
- 默认密码登录,修改密码
 
  `mysql -uroot -p`
  
- 修改密码为:123456
 
  `ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';`
  
- 如果密码设置太简单,会报以下错误

   `ERROR 1819 (HY000): Your password does not satisfy the current policy requirements`
   
- 解决该错误

  ```$xslt
    set global validate_password_policy=0; 
    set global validate_password_length=1;
  ```
  
- 再次修改密码为:123456

  `ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';`
  
- 授权其他机器使用: root/123456 登录

  `GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;`
  
- 刷新权限设置

  `FLUSH  PRIVILEGES;`
  
- 最后,去阿里控制台添加安全组规则,开发 3306 端口,,这也,,可以使用 navite 登录 mysql 了


## 参考:

- [mysql官方文档](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)