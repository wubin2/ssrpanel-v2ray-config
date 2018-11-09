### 添加节点
先在SSRPanel后台添加一个V2ray节点 （端口为10086，因为ssrpanel-v2ray里的config.json里的默认配置是10086）
### 安装JDK8
```shell
$ nano /etc/apt/sources.list.d/java-8-debian.list  #添加下面两行java源
deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main
deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main
$ apt-get install software-properties-common dirmngr  #首次安装证书需要的依存
$ apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886  #安装证书
$ apt-get update
$ apt-get install openjdk-8-jdk
```
### 安装ssrpanel-v2ray控制台和v2ray内核
```shell
$ cd /usr/local
$ wget https://github.com/aiyahacke/ssrpanel-v2ray/releases/download/0.0.2/ssrpanel-v2ray-0.0.2.zip  #ssrpanel-v2ray控制台
$ apt-get install zip  #首次安装解压软件
$ unzip ssrpanel-v2ray-0.0.2.zip -d ssrpanel-v2ray  #解压缩ssrpanel-v2ray-0.0.2.zip
$ chmod -R a+x ssrpanel-v2ray  #修改目录权限755
$ wget https://github.com/v2ray/v2ray-core/releases/download/v4.3/v2ray-linux-64.zip  #v2ray内核
$ unzip v2ray-linux-64.zip -d v2ray-linux-64  #解压缩v2ray-linux-64.zip
$ chmod -R a+x v2ray-linux-64  #修改目录权限755
$ rm -rf ssrpanel-v2ray-0.0.2.zip && rm -rf v2ray-linux-64.zip  #删除压缩包
$ cp /usr/local/ssrpanel-v2ray/config.json /usr/local/v2ray-linux-64/  #修改config.json配置后复制到内核目录
$ nano /etc/systemd/system/ssrpanel.service  #添加服务自启动
```
ssrpanel.service自启动内容如下：
```bash
[Unit]
Description=ssrpanel
After=network.target

[Service]
Type=simple
WorkingDirectory=/usr/local/ssrpanel-v2ray
ExecStart=/usr/bin/java -jar /usr/local/ssrpanel-v2ray/ssrpanel-v2ray-0.0.2.jar

[Install]
WantedBy=multi-user.target
```
```shell
$ systemctl daemon-reload  #刷新自启服务列表
$ systemctl enable ssrpanel.service  #激活ssrpanel.service服务
$ systemctl start ssrpanel.service  #启动ssrpanel.service服务
$ systemctl status ssrpanel.service  #ssrpanel.service服务状态
$ systemctl stop ssrpanel.service  #停止ssrpanel.service服务
$ systemctl restart ssrpanel.service  #重启ssrpanel.service服务
```
### 测试ssrpanel-v2ray是否安装成功
```shell
$ cd /usr/local/ssrpanel-v2ray
$ java -jar ssrpanel-v2ray-0.0.2.jar
```
### 卸载官方v2ray程序
`$ rm -rf /usr/bin/v2ray && rm -rf /usr/local/bin/v2ray && rm -rf /var/log/v2ray && rm -rf /etc/systemd/system/v2ray.service && rm -rf /etc/init.d/v2ray`

### 更新v2ray内核
备份下载配置文件 `/usr/local/v2ray-linux-64/config.json`
v2ray内核地址：https://github.com/v2ray/v2ray-core/releases
```shell
$ cd /usr/local  #程序安装目录
$ wget https://github.com/v2ray/v2ray-core/releases/download/v4.3/v2ray-linux-64.zip  #更改最新内核的下载地址
$ systemctl stop ssrpanel.service  #停止服务
$ rm -rf /usr/local/v2ray-linux-64  #删除v2ray内核目录
$ unzip v2ray-linux-64.zip -d v2ray-linux-64  #解压缩v2ray-linux-64.zip
$ rm -rf v2ray-linux-64.zip  #v2ray-linux-64.zip
```
上传 config.json 到程序安装目录`/usr/local/v2ray-linux-64/`
```shell
$ chmod -R a+x v2ray-linux-64  #修改目录执行权限
$ systemctl start ssrpanel.service  #启动服务
$ /usr/local/v2ray-linux-64/v2ray -version  #检查内核版本
$ systemctl status ssrpanel.service  #检查服务是否成功开始
```

### 其他
查看v2ray内核版本：`$ /usr/local/v2ray-linux-64/v2ray -version`
v2ray内核更新：https://github.com/v2ray/v2ray-core/releases
ssrpanel更新：https://github.com/ssrpanel/ssrpanel
ssrpanel-v2ray更新：https://github.com/aiyahacke/ssrpanel-v2ray/releases
