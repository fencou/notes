启动项

shell:startup
```
C:\Users\anyma\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
注册表
```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
新建字符串项，修改值
```



扩展屏幕 Spacedesk 

官网：https://www.spacedesk.net/zh
iOS：https://apps.apple.com/cn/app/id1069217220

麦克风WO Mic

官网：https://wolicheng.com/womic/
iOS：https://itunes.apple.com/us/app/id1260978417

远程解锁

官网：https://rfu.czqu.ren/

摄像头droidcam

官网：https://www.dev47apps.com/

## scrcpy
https://github.com/Genymobile/scrcpy/

```
scrcpy --tcpip=192.168.1.1       # default port is 5555
scrcpy --tcpip=192.168.1.1:5555
```
### scrcpy-Gui
https://github.com/Tomotoes/scrcpy-gui/blob/master/README.zh_CN.md

## Airplay-SDK
https://github.com/xfirefly/Airplay-SDK/tree/master/windows-receiver

## Win11 家庭版 RDP

每次更新版本号都有变化，需要去github搜索自己的版本号[https://github.com/stascorp/rdpwrap/issues](https://github.com/stascorp/rdpwrap/issues)

https://github.com/stascorp/rdpwrap/
搜索

![](/.media/rdp0.png)
![](/.media/rdp1.png)
![](/.media/rdp2.png)

下载 `RDP Wrapper Library v1.6.2`
双击`install.bat`文件
打开`RDPConf.exe`
![](/.media/rdp3.png)
先解决绿色区域，需要替换`C：\Program Files\RDP\`目录下的`rdpwrap.ini`文件
### 手动更新
几个源地址
https://raw.githubusercontent.com/UMU618/rdpwrap.ini/main/rdpwrap.ini
### 自动更新

红色区域
Not listening怎么办？

1. 以管理员身份启动CMD，进入命令提示符。

2. 输入`net stop termservice`回车

3. 随后替换 C:Program FilesRDP Wrapper目录中的rdpwrap.ini(用于替换的rdpwrap.ini文件在压缩包中的rdpwrap_HBM目录中)

4. 输入`net start termservice`回车，

5. 重启电脑。
### 3389端口
一般用`RDPCheck.exe`测试正常 ，远程无法访问，就是`高级安全WindowsDefender防火墙`防火钱入站规则3389端口没有创建。