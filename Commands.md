## 给虚拟机分配单个固定地址
```
nova boot --image ubuntu 18.04 --flavor m1.small --nic net-name=net_5g,v4-fixed-ip=5.5.100.2 mano_k8s_master
#指定虚拟机的镜像、实例类型名称、(虚拟机创建在的可用域名称及可用域中指定的node节点名称)、网络名称以及分配给虚拟机的固定IP地址、所创建的虚拟机名称
```
## 给虚拟机分配双网卡固定地址
```
nova boot \
--image ubuntu_5gc \
--flavor m1.samll  \
--nic net-name=net_5g,v4-fixed-ip=5.5.100.51 \
--nic net-name=net_ngap,v4-fixed-ip=5.5.200.101 \
amf
#openstack是有多用户的，网络名称可能会冲突，通过网络ID区分不会冲突；
通过 openstack network list 查看网络ID
```

## 批量创建100个虚拟机到10个node节点上
```
vim create-virtual-machine.sh
#!/bin/bash
node=1
for i in `seq 100`;do
    while true;do
        if [ $node -le 10  ];then            nova boot --image CentOS-7.2.1511-template --flavor 1C-1G-25G --availability-zone projectA:openstack-node${node}.example.local  --nic net-name=internal-net,v4-fixed-ip=10.10.7.${i} VM${i}
            node=$[node+1]
            break
         else
            node=1  
            nova boot --image CentOS-7.2.1511-template --flavor 1C-1G-25G --availability-zone projectA:openstack-node${node}.example.local  --nic net-name=internal-net,v4-fixed-ip=10.10.7.${i} VM${i}
            node=$[node+1]
            break
         fi
     done 
done
```
## 数据库操作常用的命令：
```
进入数据库：# mysql -u root -p,然后输入密码
查看数据库：# show databases;
使用数据库：# use <database name>;
查看数据库包含表：# show tables;
查看表内容：# select * from <table name>\G;
删除表中对应行的内容：# delete from <table name> where INFORMATION=**;
删除表中所有内容：# delete from <table name>;
更新表中的信息：# update from <table name> set KEY-NAME=** where INFORMATION=**;
```

## Openstack日志查看
```
Openstack的日志全部放在了systemctl里面
1. 查看各个组件的服务名
# sudo systemctl status "devstack@*"
看到tacker的服务名为：devstack@tacker.service
 
2. 查看报错的日志：
# sudo journalctl -a -u devstack@c-vol.service | grep "ERROR"
```
