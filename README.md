# Openstack_Tacker 安装教程
## 准备环境
新建虚拟机采用Ubuntu 18.04版，双网卡<br> 
界面配置第一个网卡 
```
address：192.168.210.xxx
netmask：255.255.255.0
gateway：192.168.210.254 
dns-nameserver：8.8.8.8 
```
(选择安装)<br>
网络工具：apt install net-tools 查看ifconfig 网卡信息<br>

打开SSH (https://blog.csdn.net/baidu_38407190/article/details/105981111)<br>
更换apt源（阿里源）和pip源（清华源）(https://www.cnblogs.com/cymwill/p/10293205.html)<br>
更新apt-get update, apt-get upgrade，安装 apt install python-pip <br>
安装apt-get install git <br>
## 开始安装
参看教程 https://github.com/free5gmano/tacker-example-plugin/blob/master/README.md <br>
需要更改的地方：<br>
>1.更换devstack的Git源 https://github.com/openstack/devstack.git <br>
>2.脚本文件中更换其中各个组件的Git源（到devstack中逐个查找更换）<br>
>3.注意网卡的配置信息，切记<br>
## 问题解决
https://yangsijie666.github.io/2018/09/12/devstack%E5%AE%89%E8%A3%85R%E7%89%88/ 安装教程写的不错<br>
问题1：<br>
	https://www.cnblogs.com/zpaixx/p/10578067.html中的问题1
