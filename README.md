# Openstack_Tacker 安装教程

## Table of Contents
- [准备环境](#准备环境)
- [开始安装](#开始安装)
- [问题解决](#问题解决)

## 准备环境
新建虚拟机采用Ubuntu 18.04版，双网卡<br> 
界面配置第一个网卡 （根据本地网络环境调整）
```
address：192.168.210.xxx
netmask：255.255.255.0
gateway：192.168.210.254 
dns-nameserver：8.8.8.8 
```
(选择安装)<br>
网络工具：apt install net-tools 查看ifconfig 网卡信息<br>
https://blog.csdn.net/mountzf/article/details/52035499  interfaces网卡配置详解<br>

1. 打开SSH (https://blog.csdn.net/baidu_38407190/article/details/105981111)  后面的操作可以在Xshell中进行<br>
2. 更换apt源（阿里源或者清华源）和pip源（清华源）(https://www.cnblogs.com/cymwill/p/10293205.html)<br>
3. 更新apt-get update && apt-get upgrade <br>
4. 安装 apt install python-pip <br>
5. 安装apt-get install git <br>

## 开始安装
参看教程 https://github.com/free5gmano/tacker-example-plugin/blob/master/README.md <br>
记得在stack用户下同样需要更换pip源 <br>
需要更改的地方：<br>
>1.更换devstack的Git源 https://github.com/openstack/devstack.git <br>
>2.脚本文件中更换其中各个组件的Git源（到devstack中逐个查找更换）<br>
>3.注意网卡的配置信息，切记<br>
## 问题解决
https://yangsijie666.github.io/2018/09/12/devstack%E5%AE%89%E8%A3%85R%E7%89%88/  安装教程写的不错<br>
https://blog.csdn.net/hjc121125/article/details/105519302 跟我们安装的版本一致，而且其中出现的问题很相似<br>

问题1：权限问题 <br>
>https://blog.csdn.net/hjc121125/article/details/105519302 中的问题5 <br>

问题2：虚拟环境问题<br>
>在当前文件夹下执行 # virtualenv ../requirements/.venv/<br>
>https://blog.csdn.net/zengfenliang/article/details/82875359 <br>
>https://www.cnblogs.com/zpaixx/p/10578067.html 中的问题1 <br>

问题3：requirement中hacking版本<br>
>打开并修改/opt/stack/tacker-horizon/test-requirements.txt中的版本限制，选择直接注释掉<br>

问题4：报错Unable to establish SSL connection<br>
>https://cloud.tencent.com/developer/article/1198254 中的问题2<br>

问题5：uwsgi安装时报错找不到文件或者目录<br>
>打开devstack/lib/apache，修改第97行的匹配模式uwsgi*/为*.tar.gz；修改第98行的解压路径uwsgi为$dir，如下图;<br>
![](https://github.com/meetRight/Pictures/blob/master/tacker1.png)

问题6：openstack的组件克隆问题<br>
>因为网络问题可能克隆失败，可以选择更换克隆地址，单独执行。<br>

问题7：卡在cloning Nova<br>
>换网址 https://gitee.com/sulinuxsu/nova.git 单独执行命令<br>

问题8：无法卸载某些安装包(pustil)的问题<br>
>执行命令sudo pip install XXX -U --ignore-installed<br>

问题9：openstack服务器重启后无法上网<br>
>原因是openstack将外网网卡ens3加入网桥之中，配置无法生效。<br>
>给网桥配置相关的IP地址和路由即可，具体操作如下<br>
```
ifconfig br-ex 192.168.210.XXX/24
ip route add default via 192.168.210.254 dev br-ex
```

问题10：openstack中存在界面中虚拟机、卷、网络服务等删除失败的问题，以下分类解决<br>

1. 虚拟机删除失败的问题：打开对应的nova数据库（多节点情况下找到对应的节点nova_cell**)将虚拟机对应的deleted状态置为id号即可；<br>
2. 卷服务删除的问题：先在数据库中删除对应的信息，然后将饼图（quota）中卷的deleted状态置为1；<br>
3. 网络服务无法删除的问题：包含网络服务、网络服务模板、转发图模板、和VNF。删除数据库时会遇见外键约束的问题无法删除，可以根据外键约束信息提示找到相应的依赖表，然后逐个删除。<br>


