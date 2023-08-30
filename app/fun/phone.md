
# APPs

## fooview 
中文教程https://www.kancloud.cn/sealt/fooview/

> ADB 权限提升
使用 ADB 命令提升 fooView 在手机上的权限。
请在PC端使用 ADB 来执行以下命令，手机端执行时请删除 adb shell 前缀。

辅助功能防被杀
```
提升:adb shell pm grant com.fooview.android.fooview android.permission.WRITE_SECURE_SETTINGS
恢复:adb shell pm revoke com.fooview.android.fooview android.permission.WRITE_SECURE_SETTINGS
```

## ACC电池高级管理器

[xda地址](https://forum.xda-developers.com/t/advanced-charging-controller-acc.3668427/)


Repository
[Source Code and Documentation](https://github.com/VR-25/acc/)
[Releases](https://github.com/VR-25/acc/releases/)

Front-ends
[ACC App (AccA)](https://github.com/mattecarra/acca/releases)
[ACC Settings](https://github.com/CrazyBoyFeng/AccSettings/releases)

```
battery/charge_control_limit 0 battery/charge_control_limit_max
```

## island
https://github.com/oasisfeng/island/

```
//操作是adb命令，也可以直接在手机上用终端执行。

adb devices
//检测手机是否链接调试模式

adb -d shell
//进入adb模式

su
//获取root权限，手机操作直接从这一步开始

resetprop ro.debuggable 1
//设置为全局可调式

setprop persist.sys.max_profiles 10
//配置多开用户最大值为10

am restart
//软重启

adb -d shell
//重启以后进入adb模式

su
//获得root

pm create-user --profileOf 0 --managed Island
//最后的Island为工作空间的名字，可以自己改名字，英文或者数字都可以。比如Island2一定要改不一样的

//得到的反馈是Success: created user id 11---记住这个数字
如果提示Error: couldn’t create User，不能创建用户，先执行setprop fw.max_users 10

//下面的命令里的数字11是前面取得的user id数字，
pm install-existing --user 11 com.oasisfeng.island
//为工作空间安装炼妖壶方法一

pm path com.oasisfeng.island
pm install -r --user 12 /data/app/com.oasisfeng.island-mbLCc_3ixhhZDaW2UQ0yIg==/base.apk
//为工作空间安装炼妖壶方法二


dpm set-profile-owner --user 11 com.oasisfeng.island/.IslandDeviceAdminReceiver
//设置炼妖壶拥有工作空间的管理权限，这一步设置成功以后就可以直接用炼妖壶安装软件了。

pm install-existing --user 11 com.tencent.mm
//如果上一步失败，可以直接用包名来安装软件。

am start-user 11
//开启工作空间




dpm set-profile-owner --user 0 --name Mainland com.oasisfeng.island/.IslandDeviceAdminReceiver
//主空间托管 

adb shell pm grant com.oasisfeng.island android.permission.INTERACT_ACROSS_USERS
//跨空间文件
```

## 禅定时钟
[http://quzuoqingdan.cn/](http://quzuoqingdan.cn/)

## picmarker

## 易歪歪

## cc来电拦截+

# ROMs
## https://crdroid.net/
