---
title: elasticsearch install
date: 2022/05/04
categories: 
- logs
---

下载和安装组件

确认 elasticsearch、kibana 等插件的版本保持一致

```
#elasticsearch
wget https://mirrors.tuna.tsinghua.edu.cn/elasticstack/yum/elastic-7.x/7.9.0/elasticsearch-7.9.0-x86_64.rpm

#kibana
wget https://mirrors.tuna.tsinghua.edu.cn/elasticstack/yum/elastic-7.x/7.9.0/kibana-7.9.0-x86_64.rpm

#install

sudo rpm --install elasticsearch-7.9.0-x86_64.rpm

#start
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service

#启动elasticsearch 服务
sudo systemctl start elasticsearch.service

#验证
curl 127.0.0.1:9200

#查找安装路径
whereis elasticsearch

```

这里会有两个路径

/etc/elasticsearch 是配置文件所在的路径

/usr/share/elasticsearch 运行路径

如果需要远程服务

```
# 修改 /etc/elasticsearch 配置
# network.host: 0.0.0.0

#然后重启服务
systemctl restart elasticsearch.service

#查看状态
systemctl status elasticsearch.service -l


```

启用相关端口

```
#9200 端口
sudo firewall-cmd --zone=public --add-port=9200/tcp --permanent

#重启防火墙
firewall-cmd --reload
```


# 安装kibana

```
#安装 kibana
sudo rpm --install kibana-7.9.0-x86_64.rpm

#设置开机自启
sudo systemctl daemon-reload
sudo systemctl enable kibana.service

#启动 elasticsearch 服务
sudo systemctl start kibana.service
```

kibana 是对 elasticsearch 中的数据进行可视化管理的，/etc/kibana 路径下，找到 kibana.yml 文件，配置项如下:

```
#允许远程访问
server.host: "0.0.0.0"

#设置服务的名称
server.name: "elastic-kibana"

#设置需要连接的 elasticsearch 服务地址
elasticsearch.hosts: ["localhost:9200"]

#设置页面通过中文显示
i18n.locale: "zh-CN"


#开放 5601 端口
sudo firewall-cmd --zone=public --add-port=5601/tcp --permanent

#重启防火墙
firewall-cmd --reload
```




---

错误说明

我们需要针对 elasticsearch 进行节点的相关配置，因为这里采用的只是单机单节点，并不会搭建集群，因此，重新打开 elasticsearch.yml 文件，修改如下的配置项即可

the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured

### 设置集群名称
```
cluster.name: elastic-cluster
```

### 设置节点名称
```
node.name: node-mater
```

#### 默认初始化的节点名称
```
cluster.initial_master_nodes: ["node-mater"]
```

当然，你也可以直接修改配置文件，指明当前的 elasticsearch 服务以单节点的形式运行，不过，不推荐这种方式
```
discovery.type: single-node
```






