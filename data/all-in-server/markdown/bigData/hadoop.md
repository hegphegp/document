# Hadoop

## 1. 注意点和知识点
* #### 集群中配置多个namenode和在集群中使用secondaryNamenode是完完全全的两码事，secondaryNamenode的作用是给namenode分担压力的，定时帮助namenode做一些处理。而配置多个namenode的相当于配置了一个联邦集群，每个anmenode之间都不会进行通信，各自管理各自的命名空间。
* #### 某个节点安装哪些插件，配置哪些参数才可以成为 datanode 或者是 historyserver 或者是 namenode 或者是 nodemanager 或者是 resourcemanager 
* #### 在一个Hadoop集群环境中，NameNode，SecondaryNameNode和DataNode是需要分配不同的节点上的，所以至少有三个节点
* - 集群中配置多个namenode和在集群中使用secondaryNamenode是完完全全的两码事  
* - 集群中配置多个namenode和在集群中使用secondaryNamenode是完完全全的两码事

## 2. 搭建
### Hadoop分布式集群至少需要三台服务器，以及每台机器的作用(每台机器安装的Hadoop软件都一样，只是配置参数不一样)
> 在一个Hadoop集群环境中，NameNode，SecondaryNameNode和DataNode是需要分配不同的节点上的，所以至少有三个节点

* #### 第一台用来记录所有的数据分布情况，运行的进程就是NameNode。
* #### 第二台用来备份所有数据分布情况，毕竟当前面的那台服务器宕机的时候，还可以通过该服务器来恢复数据。所以，该服务器运行的程序就是SecondaryNameNode
* #### 第三台用来存储实际的数据，运行的进程就是DataNode。
* #### 第四台是可选的服务器用来记录应用程序历史的运行状况。运行的程序就是History Server了。