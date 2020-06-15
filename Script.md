## 控制节点

```
[[local|localrc]]
enable_plugin heat https://github.com/openstack/heat.git stable/rocky
enable_plugin tacker https://github.com/openstack/tacker.git stable/rocky
enable_plugin networking-sfc https://github.com/openstack/networking-sfc.git stable/rocky
enable_plugin barbican https://github.com/openstack/barbican.git stable/rocky
enable_plugin mistral https://github.com/openstack/mistral.git stable/rocky
enable_plugin aodh https://github.com/openstack/aodh.git stable/rocky

USE_SCREEN=True
LOGFILE=/opt/stack/logs/stack.sh.log
HOST_IP=192.168.210.xxx
SERVICE_HOST=$HOST_IP
GIT_BASE=https://github.com
DATABASE_TYPE=mysql
MYSQL_HOST=$SERVICE_HOST
RABBIT_HOST=$SERVICE_HOST
GLANCE_HOSTPORT=$SERVICE_HOST:9292
ADMIN_PASSWORD=password
MYSQL_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
SERVICE_TOKEN=$ADMIN_PASSWORD

enable_service n-novnc
enable_service n-cauth
disable_service tempest

NEUTRON_CREATE_INITIAL_NETWORKS=False
DOWNLOAD_DEFAULT_IMAGES=False

Q_PLUGIN=ml2
Q_AGENT=openvswitch

disable_service etcd3

MULTI_HOST=1

FLAT_INTERFACE=ens192
PUBLIC_INTERFACE=en3160

enable_service placement-api
enable_service placement-client
```

## 计算节点

```
[[local|localrc]]
SERVICE_HOST=192.168.210.xxx
GIT_BASE=https://github.com
DATABASE_TYPE=mysql
MYSQL_HOST=$SERVICE_HOST
RABBIT_HOST=$SERVICE_HOST
GLANCE_HOSTPORT=$SERVICE_HOST:9292
KEYSTONE_AUTH_HOST=$SERVICE_HOST
KEYSTONE_SERVICE_HOST=$SERVICE_HOST
LOGFILE=/opt/stack/logs/stack.sh.log
ADMIN_PASSWORD=password
DATABASE_PASSWORD=password
RABBIT_PASSWORD=password
SERVICE_PASSWORD=password

PIP_UPGRADE=Flase

disable_service etcd3

# Neutron options
NEUTRON_CREATE_INITIAL_NETWORKS=False
MULTI_HOST=1

#---------------compute node common section
ENABLED_SERVICES=n-cpu,q-agt,n-api-meta,placement-client,n-novnc
NOVA_VNC_ENABLED=True
NOVNCPROXY_URL="http://$SERVICE_HOST:6080/vnc_auto.html"


#---------------compute node special section
HOST_IP=192.168.210.xxx
PUBLIC_INTERFACE=ens160
FLAT_INTERFACE=ens192
VNCSERVER_PROXYCLIENT_ADDRESS=$HOST_IP
VNCSERVER_LISTEN=$HOST_IP
```
