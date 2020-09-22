## docker遇到的问题

### 1. Driver overlay2 failed to remove root filesystem for id: remove /var/lib/docker/overlay2/id/merged: device or resource busy
### 1. driver "overlay" failed to remove root filesystem for id: remove /var/lib/docker/overlay/id/merged: device or resource busy
#### 1.1 描述
* 在删除docker容器的时候报错,说设备正忙  

##### 1.1.1 异常的全部信息
```
Error response from daemon: Driver overlay2 failed to remove root filesystem for id: remove /var/lib/docker/overlay2/id/merged: device or resource busy
Error response from daemon: driver "overlay" failed to remove root filesystem for id: remove /var/lib/docker/overlay/id/merged: device or resource busy
```

#### 1.2 问题现象
```
docker-compose rm -fs
docker rm -f test-mysql
# Error response from daemon: Driver overlay2 failed to remove root filesystem 21852249cce97a1ad5b631ca9f52c81b862d741591e0d337011a28d31992b64f: 
# remove /var/lib/docker/overlay2/38735aa65b119c0c9cc620e07329279bcc20e9482feaaf81d85982c5ccc4543c/merged: device or resource busy
# 
# Error response from daemon: driver "overlay" failed to remove root filesystem for 21852249cce97a1ad5b631ca9f52c81b862d741591e0d337011a28d31992b64f: 
# remove /var/lib/docker/overlay/38735aa65b119c0c9cc620e07329279bcc20e9482feaaf81d85982c5ccc4543c/merged: device or resource busy
```
#### 1.3 解决方法
##### 1.3.1 查看容器的64位ID
```
docker ps -a -q --no-trunc
# 66c1e017fa7250e9df1be26c4efbc5898e6056f91acef6453522f5d95b12879c
# 8b148d1c8fb2112abcaf9f2edb28f84baa1e7e6a42bad6f20e97c274e04d5028
# a568d71c2312366261066a228b45ab863ff83e007613596d053c5261faa47ae7
# f7e2f2ca39e13012b28cbc2e58a249cdd8a53bc63a044b46d4c69feabdf12707
# 44bc177660222e362f873355c3510cbf91828e3dd9cdd6f897345623d92ed51c
```
##### 1.3.2 通过容器的部分或者全部ID来截取设备进程的ID
```
grep docker /proc/*/mountinfo | grep 66c1e017fa7250e9df1be2 | awk -F':' '{print $1}'  |awk '!a[$1" "$2]++{print}' | awk -F'/' '{print $3}'
# grep: /proc/5637/mountinfo: 无效的参数
# grep: /proc/5667/mountinfo: 无效的参数
# grep: /proc/5673/mountinfo: 无效的参数
# 10137
# 10141
# 1014
# 10172
# 10186
# 1018
# 10206
# 1028
# 1037
# 103
# 1040
# 1042
# 10
# 11385
# 113
# 11568
# 1170
# 11
```
##### 1.3.3 将这些进程杀掉即可
```
kill -9 10137
kill -9 10141
kill -9 1014
kill -9 10172
kill -9 10186
kill -9 1018
kill -9 10206
kill -9 1028
kill -9 1037
kill -9 103
kill -9 1040
kill -9 1042
kill -9 10
kill -9 11385
kill -9 113
kill -9 11568
kill -9 1170
kill -9 11
```

### 2. docker支持的storage driver
#### 2.1 支持的storage driver
存储驱动 | 类型 | 问题指数(越小越好)
:---:|:---:|:---:
AUFS | FS | 1
BTRFS | Block | 5
DeviceMapper | Block | 6
Overlay | FS | 5
Overlay2 | FS | 3
ZFS | Block | 1
* 最好的存储方式排序 AUFS < ZFS < Overlay2 < Overlay < BTRFS < DeviceMapper  
* <b>docker资深代码贡献人员(cpuguy83)在2016年7月26日博客给出的storage driver图，该开发人员在2018-10-12那天，docker代码贡献排行第6位<b>
* <b>Centos7, Centos7.1, Centos7.2, Centos7.3, Centos7.4, Centos7.5系统安装docker，好像存储驱动不一样，装docker一定要用最新版的Centos系统，问题最少</b>

