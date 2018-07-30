### 1、配置网络

ifconfig  获取ip

连接ssh

```php
[root@zws network-scripts]# cp ifcfg-eth0 ifcfg-eth0bak
[root@zws network-scripts]# sudo vim ifcfg-eth0

DEVICE=eth0
HWADDR=00:0C:29:16:D7:95
TYPE=Ethernet
UUID=98cb3d81-6187-4380-ab68-2cb0da5faede
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.1.11
NETMASK=255.255.255.0
```

如果上不了网，重启一下，重新ifconfig获取新的地址

192.168.141.129



###  2、开机后

```php
service network restart
service network status
ifconfig //192.168.141.129
```

### 3、连接ssh

###  4、连接xftp

```php
192.168.141.129   
sftp
22
```

###  5、安装PHP

```
[root@zws softpackage]# pwd
/home/work/study/softpackage
[root@zws softpackage]# ll
总用量 11832
-rw-r--r--. 1 root root 12113028 7月  29 20:47 php-7.2.6.tar.xz
[root@zws softpackage]# tar -xvf php-7.2.6.tar.xz
[root@zws php-7.2.6]# cd /home/work/study/soft/php/
[root@zws php-7.2.6]# cd /home/work/study/soft/php/
[root@zws php]# cd /home/work/study/softpackage/php-7.2.6
```

**先要安装yum -y install gcc libxml2**

```php
yum -y install php-mcrypt libmcrypt libmcrypt-devel  autoconf  freetype gd jpegsrc libmcrypt libpng libpng-devel libjpeg libxml2 libxml2-devel
```

```php
[root@zws php-7.2.6]# ./configure  --prefix=/home/work/study/soft/php
[root@zws php-7.2.6]# make && make install
[root@zws php-7.2.6]# cd /home/work/study/soft/php/
[root@zws php]# vim text.php
[root@zws php]# ./bin/php text.php
[root@zws php]# ./bin/php -m
[root@zws php]# ./bin/php -v
```

```php
[root@zws php]# pwd
/home/work/study/soft/php
[root@zws php]# vi ~/.bash_profile
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export PATH
alias php=/home/work/study/soft/php/bin/php
[root@zws php]#  source ~/.bash_profile
[root@zws php]# cd ~
[root@zws ~]# php -v
PHP 7.2.6 (cli) (built: Jul 30 2018 00:35:23) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologie
```



###  转移php.ini文件

```php
[root@zws bin]# cd /home/work/study/softpackage/php-7.2.6
[root@zws php-7.2.6]# cp php.ini-development /home/work/study/soft/php/etc/
[root@zws php-7.2.6]# cd /home/work/study/soft/php/etc
[root@zws etc]# mv php.ini-development php.ini
[root@zws php]# php -i | grep php.ini
Configuration File (php.ini) Path => /home/work/study/soft/php/lib
[root@zws php]# mv ./etc/php.ini ./lib/
```

###  6、安装swoole

```php
[root@zws study]# yum  -y install git
[root@zws study]# git clone 
https://gitee.com/swoole/swoole.git

[root@zws study]# ll
总用量 12
drwxr-xr-x.  2 root root 4096 7月  25 09:26 demo
drwxr-xr-x.  9 root root 4096 7月  25 09:18 php
drwxr-xr-x. 13 root root 4096 7月  25 09:48 swoole
[root@zws swoole]# /home/work/study/soft/php/bin/phpize
```

#### configure时候会报错

```php
[root@zws bin]# find / -name php-config
/home/work/study/soft/php/bin/php-config
/home/work/study/softpackage/php-7.2.6/scripts/php-config
error: C++ preprocessor "/lib/cpp" fails sanity check 问题的解决
```

```php
  问题的根源是缺少必要的C++库。如果是CentOS系统，运行，如下命令解决：

  yum install glibc-headers

  yum install gcc-c++ 
```



#### phpize是用来扩展php模块的，通过phpize可以建立php的外挂模块,解决没有configure问题

```php
[root@zws swoole]# ./configure  --with-php-config=/home/work/study/soft/php/bin/php-config
[root@zws bin]# pwd
/home/study/php/bin/php-config  //php配置位置
[root@zws bin]# php -i | grep php.ini
Configuration File (php.ini) Path => /home/study/php/lib
[root@zws swoole]# make
[root@zws swoole]# make install
[root@zws lib]# cd /home/work/study/soft/php/lib/
[root@zws lib]# ll
总用量 76
drwxr-xr-x. 15 root root  4096 7月  30 00:36 php
-rw-r--r--.  1 root root 70177 7月  30 01:03 php.ini
[root@zws lib]# vi php.ini //加入extension = swoole
在php.ini文件中添加：extension=swoole.so
查看是否添加成功：php -m
在swoole/examples/server下执行php echo.php
查看是否执行端口：9501
netstat -anp|grep 9501
```

### 7、安装nginx

首先由于nginx的一些模块依赖一些lib库，所以在安装nginx之前，必须先安装这些lib库，这些依赖库主要有g++、gcc、openssl-devel、pcre-devel和zlib-devel 所以执行如下命令安装

```
$   yum install gcc-c++  

$   yum install pcre pcre-devel  

$   yum install zlib zlib-devel  

$   yum install openssl openssl--devel  
```



```php
[root@zws soft]# tar -zxf nginx-1.8.0.tar.gz
[root@zws nginx-1.8.0]# ./configure --prefix=/usr/local/nginx
[root@zws nginx-1.8.0]#make
[root@zws nginx-1.8.0]# make install
[root@zws nginx-1.8.0]# service iptables stop
[root@zws nginx-1.8.0]# netstat -tunple|grep 80
[root@zws nginx-1.8.0]# cd /usr/local/nginx/sbin/
[root@zws sbin]# ./nginx
[root@zws sbin]# netstat -tunple|grep 80
tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      0          31787      9419/nginx

http://192.168.141.129/
  显示欢迎界面
[root@zws sbin]# cd /usr/local/nginx/conf/
[root@zws conf]# cp nginx.conf nginx.confbak
[root@zws conf]# mkdir vhosts
[root@zws conf]# cd vhosts/
[root@zws vhosts]# vim default.conf
[root@zws conf]# vim nginx.conf //加入vhost/*.conf
[root@zws conf]# ../sbin/nginx -s reload
```

###  8、设置共享文件夹

```php
[root@zws ~]# cd /mnt/hgfs/myShare/swoole
[root@zws conf]# ../sbin/nginx -s reload
nginx: [error] open() "/usr/local/nginx/logs/nginx.pid" failed (2: No such file or directory)
[root@zws conf]# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
[root@zws conf]# /usr/local/nginx/sbin/nginx  -s reload
[root@zws conf]#  service iptables stop //重启之后的操作

http://192.168.141.129:8811/?m=1&n=2

```



