# 1、下载Centos

[http://mirrors.163.com/centos/7/isos/x86\_64/ CentOS-7-x86\_64-Minimal-1804.iso](http://mirrors.163.com/centos/7/isos/x86_64/ CentOS-7-x86_64-Minimal-1804.iso)

## 1-1、ifconfig

命令不存在

## 1-2、ip addr

!\[image\]\(C:/Users/care/Pictures/1/1.png\)

## 1-3、配置网络

```
[imooc@lcoalhost ~]# vi /etc/sysconf/network-scripts/ifcfg-ens33
```

```
[imooc@lcoalhost ~]# service network restart
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:18:85:e3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.141.132/24 brd 192.168.141.255 scope global noprefixroute dynamic ens33
       valid_lft 1703sec preferred_lft 1703sec
    inet6 fe80::d6eb:9653:9aa7:635d/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
[root@localhost ~]# yum install net-tools //安装为了可以使用ifconfig
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:18:85:e3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.141.132/24 brd 192.168.141.255 scope global noprefixroute dynamic ens33
       valid_lft 1703sec preferred_lft 1703sec
    inet6 fe80::d6eb:9653:9aa7:635d/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
[root@localhost ~]# yum install net-tools
```

# 2、准备工作

**   地址：**[**http://mirrors.163.com/.help/centos.html**](http://mirrors.163.com/.help/centos.html)

### 2-1-1、替换默认源

1. wget  （下载默认没有安装）
2. yum install wget
3. [http://mirrors.163.com/.help/centos.html](http://mirrors.163.com/.help/centos.html)  （忽略）
4. mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
5. \[root@localhost ~\]\# cd /etc/yum.repos.d/
6. \[root@localhost yum.repos.d\]\# wget [http://mirrors.163.com/.help/CentOS7-Base-163.repo](http://mirrors.163.com/.help/CentOS7-Base-163.repo)
7. 运行以下命令生成缓存  

```
   yum clean all

   yum makecache
```

!\[image\]\(C:/Users/care/Pictures/1/3.png\)

## 2-1-2. 安装vim

1. \[root@localhost yum.repos.d\]\# yum install vim
2. 安装成功后的验证

   \[root@localhost yum.repos.d\]\# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3

   可以打开即为正常

![](/assets/import.png)

