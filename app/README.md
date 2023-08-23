## 前言

使用[迎洽AI机器人] 前，请先下载并安装以下两个客户端程序
可在自用电脑上安装（推荐），亦可购买Windows服务器安装

1. [![](https://img.shields.io/badge/%E6%AD%A3%E7%89%88%E5%BC%80%E5%8F%91-2.4.19-yellow?style=flat-square&logo=windows) 迎洽AI机器人](https://lbb918.oss-cn-shenzhen.aliyuncs.com/bot/app.zip)

2. [企业微信 WeCom_4.1.2.6014.exe](https://dldir1.qq.com/wework/work_weixin/WeCom_4.1.2.6014.exe) (切记登陆后关闭自动升级)

温馨提示：

由于程序协议有自动执行的命令，部分杀毒软件可能会报毒，找回隔离文件再将文件加入信任名单即可。

![](/.media/1.11image1.png)


## 安装机器人

文字表述如下：
1、安装 企业微信 WeCom_4.1.2.6014.exe  版本的企业微信

![](/.media/1.11image2.png)

2、安装完毕后，用机器人[企微员工]账号扫码登录，然后点开【控制台 - 设置 - 通用设置】找到软件更新这一项，关闭自动更新。

![](/.media/1.11image3.png)


切记，一定要关闭自动更新

切记，一定要关闭自动更新

如果不幸升级了，请卸载重装


## 配置

1、解压“迎洽AI机器人.zip”



2、打开config.ini 文件，选择打开方式：记事本


3、在config.ini配置文件中“TOKEN=”后面填上token（联系你的客户经理领取token）填入token ，保存文件。
** OPENAI_KAY 需要用到ChatGPT营销功能才需要填上，不用的话可以留空不填
```
[DEFAULT]
TOKEN=
OPENAI_KEY=
```

4、进入系统前台-迎洽AI机器人模块--绑定账号（这里的token需要跟config.ini文件中绑定一致）


![](/.media/1.11image4.png)

5、config.ini和系统前台配置完毕后，双击打开 app.exe ，显示企业微信登录页面，扫码登录（登录的企微就是机器人）



显示 "微信连接成功" 表示已经在正常运行了。

![](/.media/1.11image5.png)

1. Ping在线测试用于检测机器人是否正常在线
2. 点击自动更新机器人将更到最新版本，无需重复下载【迎洽AI机器人软件安装包】
3. 机器人不运行或报错，可打开调试模式自行查看原因



## 常见问题与技巧

当程序发生以下问题时：
● 打开程序提示企微已登录
● 机器人发送多次消息
● 程序多次重启

是因为有进程没有完全退出导致，可以“Ctrl+Alt+Del”打开“任务管理器”找到程序强制结束进程即可解决

![](/.media/1.11image6.png)