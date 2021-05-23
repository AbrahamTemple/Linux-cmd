# Linux命令

- 查看指定端口占用情况并将其杀掉

``` shell
[root@hecs-x-medium-2-linux-20210511093644 bin]# lsof -i:8848
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    3488 root   95u  IPv6 672520      0t0  TCP *:8848 (LISTEN)
java    3488 root  103u  IPv6 672525      0t0  TCP hecs-x-medium-2-linux-20210511093644:8848->hecs-x-medium-2-linux-20210511093644:34726 (ESTABLISHED)
java    3488 root  104u  IPv6 672526      0t0  TCP hecs-x-medium-2-linux-20210511093644:34726->hecs-x-medium-2-linux-20210511093644:8848 (ESTABLISHED)
[root@hecs-x-medium-2-linux-20210511093644 bin]# kill 3488
```

- 查看指定端口进程情况

``` shell
[root@hecs-x-medium-2-linux-20210511093644 bin]# netstat -tunlp |grep 3306
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      19774/mysqld   
```

- 按进程名搜索该进程占用情况

``` shell
[root@hecs-x-medium-2-linux-20210511093644 bin]# ps -ef|grep mysql
root      3973  2288  0 15:23 pts/0    00:00:00 grep --color=auto mysql
polkitd  13722 13702  0 May18 ?        00:02:52 mysqld
root     13840 13831  0 May18 pts/0    00:00:00 mysql -uroot -px x
polkitd  14035 14015  0 May18 ?        00:02:48 mysqld
root     14156 14148  0 May18 pts/0    00:00:00 mysql -uroot -px x
root     19207     1  0 May13 ?        00:00:00 /bin/sh /www/server/mysql/bin/mysqld_safe --datadir=/www/server/data --pid-file=/www/server/data/hecs-x-medium-2-linux-20210511093644.pid --sql-mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
mysql    19774 19207  0 May13 ?        00:04:11 /www/server/mysql/bin/mysqld --basedir=/www/server/mysql --datadir=/www/server/data --plugin-dir=/www/server/mysql/lib/plugin --user=mysql --sql-mode=NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION --log-error=hecs-x-medium-2-linux-20210511093644.err --open-files-limit=65535 --pid-file=/www/server/data/hecs-x-medium-2-linux-20210511093644.pid --socket=/tmp/mysql.sock --port=3306
```

- 查询防火墙是否开启指定端口并开放与重载

``` shell
[root@hecs-x-medium-2-linux-20210511093644 bin]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2021-05-13 02:08:06 CST; 1 weeks 3 days ago
     Docs: man:firewalld(1)
 Main PID: 27395 (firewalld)
    Tasks: 2
   Memory: 27.3M
   CGroup: /system.slice/firewalld.service
           └─27395 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid
[root@hecs-x-medium-2-linux-20210511093644 bin]# firewall-cmd --query-port=8848/tcp
no
[root@hecs-x-medium-2-linux-20210511093644 bin]# firewall-cmd --add-port=8848/tcp --permanent
success
[root@hecs-x-medium-2-linux-20210511093644 bin]# firewall-cmd --reload
success
[root@hecs-x-medium-2-linux-20210511093644 bin]# firewall-cmd --query-port=8848/tcp
yes
```
