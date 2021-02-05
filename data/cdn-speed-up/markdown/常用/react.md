## react学习


### 1.0 Ubuntu下，node的安装和配置
#### 1.1 node的安装
##### 1.1.1 方法一:二进制在线下载安装
* 可以多用户使用，root用户和其他用户共用nodejs这个软件，但是安装cnpm插件要加sudo增强命令  

```
echo "deb http://mirrors.aliyun.com/ubuntu/ $(lsb_release -cs) main restricted universe multiverse" > /etc/apt/sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ $(lsb_release -cs)-security main restricted universe multiverse" >> /etc/apt/sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ $(lsb_release -cs)-updates main restricted universe multiverse" >> /etc/apt/sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ $(lsb_release -cs)-proposed main restricted universe multiverse" >> /etc/apt/sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ $(lsb_release -cs)-backports main restricted universe multiverse" >> /etc/apt/sources.list

# 下面这句一定要运行，否则会认为国内node加速下载地址是不可信，导致不在国内加速器下载最新版本
curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | sudo apt-key add -

echo "deb https://mirrors.tuna.tsinghua.edu.cn/nodesource/deb_8.x $(lsb_release -cs) main" > /etc/apt/sources.list.d/nodesource.list
echo "deb-src https://mirrors.tuna.tsinghua.edu.cn/nodesource/deb_8.x $(lsb_release -cs) main" >> /etc/apt/sources.list.d/nodesource.list
cat /etc/apt/sources.list.d/nodesource.list 
apt-get update
apt-get install nodejs
```

##### 1.1.2 方法二：压缩包下载安装
* 可以多用户使用，/etc/profile的环境变量是所有用户共用的  

```
# https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/ 查看nodejs的安装包
mkdir -p /opt/soft/nodejs
curl -L http://cdn.npm.taobao.org/dist/node/v8.15.1/node-v8.15.1-linux-x64.tar.gz > /opt/soft/nodejs/node-v8.15.1-linux-x64.tar.gz
tar -zxvf /opt/soft/nodejs/node-v8.15.1-linux-x64.tar.gz -C /opt/soft/nodejs
echo "export NODE_HOME=/opt/soft/nodejs/node-v8.15.1-linux-x64" >> /etc/profile
echo 'export PATH=$NODE_HOME/bin:$PATH' >> /etc/profile
source /etc/profile
node -v
npm -v

# /opt/soft/nodejs/node-v8.15.1-linux-x64/bin/node -v
# cd /opt/soft/nodejs/node-v8.15.1-linux-x64/bin && ./node -v
```

#### 1.2 node的配置
##### 1.2.1 配置淘宝源
```
sudo npm config set registry https://registry.npm.taobao.org -verbose
```

##### 1.2.2 安装cnpm
* 用cnpm替代npm，cnpm是阿里定制的命令行工具，用来代替默认的npm  

```
# 普通用户加sudo才可以运行，因为安装路径可以在/usr/lib/node_modules下，普通用户没权限
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org --verbose
cnpm -v
```

##### 1.2.3 安装yarn(用yarn替代npm)
```
# 普通用户加sudo才可以运行，因为安装路径可以在/usr/lib/node_modules下，普通用户没权限
# sudo cnpm install -g yarn
# sudo yarn global add create-react-app
sudo npm install -g yarn --verbose
```

#### 1.3 创建React项目的方式
##### 1.3.1 方式一:create-react-app脚手架创建
```
sudo npm install -g create-react-app --verbose
# create-react-app 项目名称
# cd 项目目录
# npm start       # 或者 yarn start 运行
# npm run build   # 或者 yarn build 生成项目
```

##### 1.3.2 方式二:npx命令创建
```
# 先安装 create-react-app 脚手架
sudo npm install -g create-react-app --verbose
# 去到项目目录运行下面命令
# npx create-react-app 项目名称
```