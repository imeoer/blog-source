title: Linux下的Android刷机
date: 2015-09-26 10:00:00
author: me
tags:
    - Linux
    - Android
    - 开发
preview: 因为开发与日常用系统都是Linux，偶尔需要给自己的Android手机刷机，但国内各大论坛上给出的刷机工具大都是Windows版的，在虚拟机下操作又不太方便，于是打算在Linux下刷机，经过一时半会的折腾，记录下具体的操作步骤

---

因为开发与日常用系统都是Linux，偶尔需要给自己的Android手机刷机，但国内各大论坛上给出的刷机工具大都是Windows版的，在虚拟机下操作又不太方便，于是打算在Linux下刷机，经过一时半会的折腾，记录下具体的操作步骤。

### 启用USB调试

打开`设置`>`关于手机`，点击`版本号`7次，回退至`设置`可以看到菜单中出现`开发者选项`，进入后打开`USB调试`选项。

### 创建USB配置

以Root权限创建`/etc/udev/rules.d/51-android.rules`文件，内容为：

``` shell
SUBSYSTEM=="usb" ENV{DEVTYPE}=="usb_device", MODE="0666"
```

执行`sudo chmod a+r /etc/udev/rules.d/51-android.rules`命令为该配置文件增加读权限。

执行`sudo /etc/init.d/udev restart`使配置文件生效。

### 查看设备ID号

用USB线将手机连接后，执行`lsusb`命令查看连接的USB设备信息：

```shell
...
Bus 001 Device 013: ID 2a70:9011
...
```

找到手机设备的一行，在这里`2a70`就是设备ID，将`0x2a70`追加到`~/.android/adb_usb.ini`文件中。

### 安装ADB与FASTBOOT命令行工具 (Ubuntu)

ADB(Android Debug Bridge)是一个开发工具，帮助Android设备与PC通信，使用USB线或WIFI方式连接后可以用它操作手机。

Fastboot是一种用USB线连接的刷机方式(线刷)，它比Recovery(卡刷)更底层。

``` shell
sudo add-apt-repository ppa:nilarimogard/webupd8
sudo apt-get update
sudo apt-get install android-tools-adb android-tools-fastboot
```

### 连接测试

用USB线将手机连接后，执行`sudo adb devices`，可以看到：

``` shell
* daemon not running. starting it now on port 5037 *
* daemon started successfully *
List of devices attached

0403502001011000    device
```

若看到`no permissions`，需要在手机上勾选`允许连接USB调试`再试。

### 愉快的玩机

万事具备，接下来就可以用`adb`与`fastboot`愉快的玩机啦，以下几个有趣的命令：

- 安装应用：`adb install -r <name>.apk`
- 执行Shell命令：`adb shell <command>`
- 模拟按键：`adb shell input keyevent <value>`
- 截屏：`adb shell screencap <path>.png`
- 录像：`adb shell screenrecord <path>.mp4`

### 常用刷机步骤

- 在开机状态下执行`adb reboot bootloader`命令，在手机重启后进入Fastboot模式。
- 在Fastboot模式下执行`fastboot devices`查看连接的手机。
- 执行`fastboot flash recovery <recovery_path>.img`命令刷入Recovery。
- 执行`fastboot boot <recovery_path>.img`命令免刷进入Recovery

使用许多第三方的Recovery就可以试试各种刷机啦。
