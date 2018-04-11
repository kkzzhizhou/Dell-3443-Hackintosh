# Hackintosh on Dell-3443
## 硬件清单
- CPU: i5-5200U(Broadwell)
- 显示屏：LG 1080P（自己换的）
- 主板型号：9CC3
- 核显: HD5500
- 独显：Nvidia 820M（无法驱动）
- 硬盘：三星850EVO SSD+原配机械硬盘500G（光驱丢掉）
- 声卡：ALC255+Intel BroadWell HDMI
- 有线网卡：Realtek 8106E
- 无线网卡：BCM94352HMB（自己换的）
- 蓝牙：20702A3（自己换的）
- 读卡器：Realtek USB2.0 Card-Reader（无法驱动）
- USB:一个USB3.0+两个USB2.0
- 摄像头：集成摄像头
- 触控板：Symthetic

## Clover版本：4411
## MacOS版本：10.12.6
- 10.13.X 94352HMB无法驱动（3404，13d3半高卡）
## 声卡：AppleHDA Patcher 1.8仿冒
## 安装在S/L/E下的驱动
- FakePCIID_Broadcom_WiFi.kext（无线网卡驱动）
- FakePCIID.kext（无线网卡驱动）
- CodecCommander.kext（解决睡眠唤醒无声）
- BrcmPatchRAM2.kext（蓝牙驱动）
- BrcmFirmwareRepo.kext（蓝牙驱动）
- AppleHDA.kext（仿冒驱动）
- ACPIBatteryManager.kext（电量显示）

## DSDT打补丁
- [igpu] Brightness fix（亮度调节补丁）
- Fix ADBG Error(解决编译报错)
- Fix PARSEOP_ZERO Error(aggressive)(解决编译报错)
- IRQ Fix(修复声卡找不到的问题)
- USB3_PRW 0x0D(instand wake)(修复睡眠问题)

## APCI/patch目录下的作用
- DSDT.aml //已打补丁的DSDT
- SSDT-Disable_DGPU.aml（禁用optimus技术的独显）
- SSDT-HDMI-HD5500（开启HDMI音频）
- SSDT-UIAC-ALL（USB内建，配合USBinjectall.kext使用）

## 解决开机第二屏花屏
Clover中注入EDID,教程：http://bbs.pcbeta.com/viewthread-1762479-1-1.html
## 白果三码（不能登录imessage和facetime的问题）
向身边有Mac电脑中提取。如果身边的朋友不使用Mac系统，可以将[OSX PE](https://www.firewolf.science/firewolf-os-x-pe-v7-cn/)安装到U盘，然后运行iMessage Debug提取。
## 键盘问题
使用Keyboard Maestro自定义。

## 黑果相关网站和QQ群
- [远景论坛](http://bbs.pcbeta.com/)
- [tonymacx86](http://www.tonymacx86.com/)
- 一起黑苹果[QQ群]：688324116

## 无解
1. 独立显卡（使用optimus技术的显卡无法驱动）
2. 读卡器（能显示为USB2.0-CRW）

## 暂时无解
1. PowerNap
2. 耳机麦克风输入
3. 睡眠唤醒后，连接路由器Samba服务慢，需要重新断开连接电源适配器
    - 10.13.X正常


