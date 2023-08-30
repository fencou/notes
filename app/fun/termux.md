
主要参考这两个博客

基础https://www.sqlsec.com/2018/05/termux.html

实战https://blog.csdn.net/YiBYiH?type=blog

## Termux 简介

[Termux 官网](https://termux.com/)
[F-Droid 下载地址](https://f-droid.org/packages/com.termux/)

Termux 是一个 Android 下一个高级的终端模拟器，开源且不需要 root，支持 apt 管理软件包，十分方便安装软件包，完美支持 Python、 PHP、 Ruby、 Nodejs、 MySQL 等。随着智能设备的普及和性能的不断提升，如今的手机、平板等的硬件标准已达到了初级桌面计算机的硬件标准，用心去打造 DIY 的话完全可以把手机变成一个强大的极客工具。

## 基本操作
### 长按屏幕
长按屏幕会调出显示菜单项（包括复制、粘贴、更多），方便我们进行复制或者粘贴：
`More` 菜单的说明如下：
```
长按屏幕
├── COPY:    # 复制
├── PASTE:   # 粘贴
├── More:    # 更多
   ├── Select URL:             # 提取屏幕所有网址
   └── Share transcipt:        # 分享命令脚本
   └── Reset:                  # 重置
   └── Kill process:           # 杀掉当前会话进程
   └── Style:                  # 风格配色 需要自行安装
   └── Keep screen on:         # 保持屏幕常亮
   └── Help:                   # 帮助文档
```
### 会话管理
显示隐藏式导航栏，可以新建、切换、重命名会话 session 和调用弹出输入法：

## 基础知识
### 基本命令
```
pkg search <query>              # 搜索包
pkg install <package>           # 安装包
pkg uninstall <package>         # 卸载包
pkg reinstall <package>         # 重新安装包
pkg update                      # 更新源
pkg upgrade                     # 升级软件包
pkg list-all                    # 列出可供安装的所有包
pkg list-installed              # 列出已经安装的包
pkg show <package>              # 显示某个包的详细信息
pkg files <package>             # 显示某个包的相关文件夹路径
```
### 目录结构
```
echo $HOME  //这个也写作 ~/
/data/data/com.termux/files/home

echo $PREFIX
/data/data/com.termux/files/usr

echo $TMPPREFIX
/data/data/com.termux/files/usr/tmp/zsh
```
## 进阶配置
### 换源
可以使用如下命令自动替换官方源为 TUNA 镜像源

> pkg update 卡住的话多按几次回车 不要傻乎乎的等
```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list

sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list

sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list

pkg update
```

> 手动换源
```
termux-change-repo
```

## 开发环境
### Mariadb
```
pkg install mariadb
```
启动
```
mysqld
```
默认的两个用户$(whoami)、root，都没有密码，使用root登录
```
mysql -u root
```
设置密码
```
set password for 'root'@'localhost' = password('你设置的密码');

# 刷新权限 并退出
flush privileges;
quit;
```
![](/termux_mariadb1.png)
创建数据库
```
CREATE DATABASE wordpress;
```
给予远程访问权限
```
grant all on *.* to root@'%' identified by 'P@ssw0rd' with grant option;
flush privileges;
```
### php
```
pkg install php
```
### php-fpm
```
pkg install php-fpm
```
启动，运行两次
```
php-fpm
```
获得
```
/data/data/com.termux/files/usr/var/run/php-fpm.sock
```

### Nginx
安装
```
pkg install nginx
```
启动、重启
```
nginx

#重启
nginx -s reload 

#停止
nginx -s stop
nginx -s quit

#杀进程
kill -9 `pgrep nginx`
```

#### 解析PHP
编辑 php-fpm 的配置文件 www.conf：
```
vim $PREFIX/etc/php-fpm.d/www.conf
```
定位搜索 listen = 找到
```
listen = /data/data/com.termux/files/usr/var/run/php-fpm.sock
```
将其改为：
```
listen = 127.0.0.1:9000
```
#### 配置 Nginx
编辑 Nginx 的配置文件 nginx.conf：
```
vim $PREFIX/etc/nginx/nginx.conf
```
1. 添加 index.php 到默认首页的规则里面：
2. 取消 location ~ \.php$ 这些注释，按照图片上的 提示修改：
3. Termux 里面的 Nginx 默认网站的根目为:

如果没有修改php-fpm配置文件，也可以将
```
fastcgi_pass   unix:/data/data/com.termux/files/usr/var/run/php-fpm.sock;
```
#### 监听端口
```
#增加ipv6
listen       [::]:8080 ipv6only=on;
```
#### wordpress伪静态
写在server大括号内
```
location ~ /
        {
             try_files $uri $uri/ /index.php?$args;
        }

        rewrite /wp-admin$ $scheme://$host$uri/ permanent;
```

### library “libssl.so.1.1“ not found
安装openssl1.1
先搜索
```
pkg search openssl1.1
```
可以看到有一个openssl1.1-tool的package，对它进行安装
```
pkg install openssl1.1-tool
```
搜索libssl.so.1.1
可以pwd先看下自己的目录
```
pwd
```
一般来说都安装到了`/data/data/com.termux/files`下

搜索
```
find /data/data/com.termux/files -name 'libssl.so.*'
```

添加环境变量
```
echo "export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib/openssl-1.1" >> ~/.bashrc
```
使当前shell生效
```
export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib/openssl-1.1
```
再次检验命令 ————成功


## termux-services

官方Wilki: [https://wiki.termux.com/wiki/Termux-services](https://wiki.termux.com/wiki/Termux-services)

To install termux-services, run

```
pkg install termux-services
```
and then restart termux so that the service-daemon is started.

如果官方提供，则按官方，比如`sshd`、`nginx`、`mysqld`
To then enable and run a service, run

```
sv-enable <service>
```
If you only want to run it once, run

```
sv up <service>
```
To later stop a service, run:

```
sv down <service>
```
Or to disable it

```
sv-disable <service>
```
A service is disabled if `$PREFIX/var/service/<service>/down` exists, so the `sv-enable` and `sv-disable` scripts touches, or removes, this file.

如果官方不提供，可按这个执行，如`php-fpm`
termux-services uses the programs from runit to control the services. A bunch of example scripts are available from the same site. If you find a script you want to use, or if you write your own, you can use set it up by running:

```
mkdir -p $PREFIX/var/service/<PKG>/log
ln -sf $PREFIX/share/termux-services/svlogger $PREFIX/var/service/<PKG>/log/run
```
and then put your run script for the package at $PREFIX/var/service/<PKG>/run and make sure that it is runnable.

You can then run

```
sv up <PKG>
```
to start it.

Log files for services are situated in $PREFIX/var/log/sv/<PKG>/ with the active log file named "current".

如果是自己创建的，则需要创建run文件
```
vim $PREFIX/var/service/<PKG>/run
```
写入内容，一个可执行文件
```
#!/data/data/com.termux/files/usr/bin/sh
exec 2>&1
exec $PREFIX/var/service/dynv6/dynv6.sh 域名 令牌 同步间隔时间 2>&1
```




## 在Tremux上配置DDNS

先安装依赖包:
```
pkg install curl -y
```
curl用来调用API向dnyv6传递ipv6地址。
```
pkg install iproute2
```
获取IPV6地址
```
ip -6 addr list scope global |grep "inet6" | sed -n 's/.*inet6 \([0-9a-f:]\+\).*/\1/p' | head -n 1
```

① 手动向dnyv6传递IP地址
```
 curl --silent 'http://dynv6.com/api/update?hostname=域名&token=令牌&ipv6='$(ip -6 addr list scope global |grep "inet6" | sed -n 's/.*inet6 \([0-9a-f:]\+\).*/\1/p' | head -n 1)
 ```

其中域名和令牌是刚刚第一步最后让你记下的。如果一切正常的话应该能看见addresses updated的执行结果了：
如果没有结果输出，检查一下命令有没有复制错，令牌和域名有没有填写正确。

然后再次登录：https://dynv6.com/ ，依次点击My Zones -> 你的域名 -> Records，就能在下方看见刚刚传递的ipv6地址了。

创建定时查询IPV6地址的可执行文件
```
vim $PREFIX/var/service/dynv6/dynv6.sh
```
写入定时更新程序
```shell
#!/data/data/com.termux/files/usr/bin/sh

time=$3
token=$2
name=$1

while true
do
	curl --silent  'https://dynv6.com/api/update?hostname='$name'&token='$token'&ipv6='$(ip -6 addr list scope global |grep "inet6" | sed -n 's/.*inet6 \([0-9a-f:]\+\).*/\1/p' | head -n 1)
	echo -n "!\t"
	ip -6 addr list scope global |grep "inet6" | sed -n 's/.*inet6 \([0-9a-f:]\+\).*/\1/p' | head -n 1
	sleep $time
done
```
可执行权限
```shell
chmod +x $PREFIX/var/service/dynv6/dynv6.sh
chmod +x $PREFIX/var/service/dynv6/run
```
最后
```shell
sv-enable dynv6
```

## 80端口转发
### 设置监听转发
安装root
```
pkg install root-repo
```
进入root权限
```
su
```
安装ip套件
```
pkg install iptables
```

```
ip6tables -t nat -I PREROUTING -p tcp --dport 80 -j DNAT --to [IPV6地址]:22
ip6tables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-port 8080
ip6tables -t mangle -A PREROUTING -p tcp --dport 80 -j TPROXY --on-port 8080

#外部端口转发,即修改来源于设备外部的访问请求
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
#内部端口转发，即修改来源于设备内部的访问请求
iptables -t nat -A OUTPUT -p tcp -d 127.0.0.1 --dport 80 -j REDIRECT --to-ports 8080

#查询
iptables -L OUTPUT  -t nat -n --line-number 
iptables -L PREROUTING  -t nat -n --line-number 
#删除规则
iptables  -t nat -D PREROUTING 1  
iptables  -t nat -D OUTPUT 1
ip6tables  -t mangle -D PREROUTING 6
```

### 添加host
通过手机中部署的clash服务来间接上网，clash的配置文件中可以使用hosts属性来起到类似修改hosts的目的。
```
hosts:
  clash: 127.0.0.1
```