#
   golang binlog example
#

###
创建mysql环境
docker run --name mysql-service -v /data/mysql:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7 
###
开启binlog
docker exec mysql-service bash -c "echo 'log-bin=/var/lib/mysql/mysql-bin' >> /etc/mysql/mysql.conf.d/mysqld.cnf"
###
设置mysql的serverid（非必须步骤）
docker exec mysql-service bash -c "echo 'server-id=1' >> /etc/mysql/mysql.conf.d/mysqld.cnf"
###
重启mysql
docker restart mysql-service

###
###
SQL

检查mysql的binlog是否开启
show variables like 'log_bin';

USE Test;

CREATE DATABASE Test；

CREATE TABLE `User` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`name` varchar(255) DEFAULT NULL,
`status` varchar(255) DEFAULT NULL,
`created` timestamp NULL DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=latin1;

INSERT INTO `User`(`name`,`status`,`created`) VALUES ("x","st",NOW());

小贴士：
你可以通过 SHOW VARIABLES LIKE 'character%' 查看字符集是否更改为utf8mb4,
也可以通过SHOW VARIABLES LIKE '%time_zone%' 查看时区是否是东八区