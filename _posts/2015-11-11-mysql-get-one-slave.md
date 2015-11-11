---
layout: post
title: "Mysql 搭建从库"
date: "2015-11-11"
categories: sddtc work
tags: [mysql]
---

####准备环境：  
一台没开过bin-log的mysql数据库实例，在此基础上给该实例做两个数据库的从库  
并且不能停库，其中一个库有50G，是innodb引擎  
本来是拜托运维负责，但是由于使用环境特殊性自己做了尝试，在此记录一下过程  
并且做完之后还有一些问题，需要后续解决  

一、停止主库消息写入任务，保证该库只有读取任务，开启bin-log配置，重启主库  
修改/etc/my.cnf文件：  

```

log-bin=mysql-bin
server-id=228
binlog-do-db=databaseone
binlog-do-db=databasetwo
expire-logs-days=7
binlog_cache_size = 4M
max_binlog_size = 300M

```

二、使用"show master status;"可以看到bin-log的position位置，这个位置对于没有开过bin-log的数据库可以不用特意记录数据变动起始位置，从库从0开始设置就可以，这时主库可以恢复一切写任务      

三、使用"mysqldump"命令导出整个库文件，并scp到从库服务器，再用"mysql"导入数据库中  
因为索引文件也很大，最耗时的操作是数据库导入，50G导出的sql导入用了至少5小时  

四、从库进行配置  
修改/etc/my.cnf文件:

```

server-id=116
master-port=3306
replicate-do-db=databaseone
replicate-do-db=databasetwo
slave-skip-errors = 1032,1062,126,1114,1146,1048,1396,1548
master-host=xx.xx.xxx.xxx
master-user=username
master-password=passport

```

需要在主库建立一个专门向从库进行进行操作的用户  

####注意事项：  
1.存储过程对表的操作，没法同步到从库中  
主要原因是主库版本非正式版，5.1.73，从库搭建选择的是官方版5.1.72  
1548的错误就是从库同步存储过程报错的错误id：  

```

Master_SSL_Verify_Server_Cert: No
Last_IO_Errno: 0
Last_IO_Error: 
Last_SQL_Errno: 1548
Last_SQL_Error: Error 'Cannot load from mysql.proc. The table is probably corrupted' on query. 
Default database: 'xxx'. Query: 'drop PROCEDURE if exists `xxxx`.`proc_xxx`


```

上网查了原因，却也解决不了问题，但是和5.1.72和之后版本的存储过程字段类型改变有关的样子。。  


2.导出sql的时候，有些建表语句写的不规范，故意将列类型指定为'gb2312',导致写入时报错，对于这种大型文件，utf8为基准的sql解决方法就是把个别这种列类型改掉，以后要提前确认问题，避免这种错误  


参考文章：  
1.[主MySQL设置](http://faq.comsenz.com/library/system/serviceext/serviceext_slave.htm)  
2.[MySQL主库已经存在的基础上搭建从库的过程--> （旧资料整理）](http://blog.csdn.net/mchdba/article/details/11354771)