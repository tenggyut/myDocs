##MySQL

###Using RPM to install MySQL 5.7

```
wget http://dev.mysql.com/get/mysql57-community-release-el6-8.noarch.rpm
yum -y install mysql mysql-devel
```

###赋权

```
grant all privileges on metastore.* to idcube@'%' identified by 'idcube';
```

###初始化一个root无密码的mysql server

```
/usr/sbin/mysqld --initialize-insecure --datadir="$datadir" --user=mysql
```

###Create Database with desired encoding 

```
create database xx DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
```

```
set global validate_password_policy=0;
set global validate_password_length=1
```
