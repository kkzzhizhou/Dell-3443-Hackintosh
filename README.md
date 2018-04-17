# Hackintosh on Dell-3443
## 硬件清单
- CPU: i5-5200U(Broadwell)
- 显示屏：LG 1080P（官方是720P屏幕，自己换了1080P的屏幕）
- **核显: HD5500**
- 独显：Nvidia 820M（无法驱动）
- 硬盘：250G 三星850EVO SSD + 500G 机械硬盘（拆了光驱）
- **声卡：ALC255**(以及核显自带的Intel BroadWell HDMI)
- 有线网卡：Realtek 8106E
- **无线网卡：BCM94352HMB**（官方的是AR9565，无法连接手机热点，换成BCM94352HMB半高卡）
- 蓝牙：20702A3（BCM94352HMB带蓝牙4.0，支持Handoff）
- 读卡器：Realtek USB2.0 Card-Reader（0x0bda,0x0129无法驱动）
- USB接口:一个USB3.0+两个USB2.0
- 摄像头：集成摄像头
- 触控板：Symthetic

## Clover版本：4439
## MacOS版本
- 2018-4-16到现在   High Sierra(10.13.4)
- 2018-4-15之前:    Sierra(10.12.6)

## Clover/ACPI/patched目录下各文件的作用
- DSDT.aml(打过补丁的DSDT,具体说明见**DSDT打补丁**)
- SSDT-Disable_DGPU.aml(禁用optimus技术的独显，解决安装10.13.X时service ran for 0 second)
- SSDT-HDMI-HD5500.aml（开启HDMI音频补丁）
- SSDT-UIAC-ALL.aml（USB内建，配合USBinjectall.kext使用）

## Clover/kexts目录下各文件的作用
- ACPIBatteryManager.kext(电源驱动，用于电量显示)
- AirportBrcmFixup.kext(BCM94352HMB WiFi驱动，需要在config.plist启动参数中添加brcmfx-country=#a -brcmfxbeta -lilubeta才能正常工作。)
- ApplePS2SmartTouchPad.kext(键盘，触摸板和鼠标驱动，修改过后接近白苹果的操作体验)
- BrcmFirmwareData.kext和BrcmPatchRAM2.kext（蓝牙驱动，如果卡在Fail to write to pulk pipe，需要添加启动参数dart=0以及Drop掉DMAR表，这个与VT-d虚拟技术有关。）
- BT4LEContiunityFixup.kext(解决Handoff，Hotspot问题)
- FaceTime_WebCam.kext(仿冒摄像头Facetime内建摄像头)
- FakeSMC.kext和Lilu.kext（黑苹果必备驱动）
- IntelGraphicsDVMTFixup.kext（解决某些笔记本DVMT内存问题）
- IntelGraphicsFixup.kext(英特尔核显驱动)
- RealtekRT8100.kext（有线网卡8106E驱动）
- USBinjectAll.kext(配合SSDT-UIAC-ALL.aml屏蔽无用的USB端口，内建USB设备)

## Clover/drivers64UEFI目录下特定文件的作用，其他于Clover自带。
- APFS.efi(安装10.13必备驱动)
- OSxAptioFixDrv-64.efi(解决卡+号的问题)
## 声卡：AppleHDA Patcher 1.8仿冒
## 显卡：在config.plist中勾选Inject Intel，并选择注入的Platform-id为16260006
## DSDT打补丁
- [igpu] Brightness fix（亮度调节补丁）
- Fix ADBG Error(解决编译报错)
- Fix PARSEOP_ZERO Error(aggressive)(解决编译报错)
- IRQ Fix(修复声卡找不到的问题)
- USB3_PRW 0x0D(instand wake)(修复睡眠问题)

## 键盘问题
使用Keyboard Maestro自定义。

## 黑果相关网站和QQ群
- [远景论坛](http://bbs.pcbeta.com/)
- [tonymacx86](http://www.tonymacx86.com/)
- 一起黑苹果[QQ群]：688324116
## 可能遇到的各种疑难杂症
### 1. 睡眠唤醒后，网卡延迟增高，需要断开重连电源适配器才能正常工作
解决方法: 这个总是在10.12.X中出现，原因是当插着电源适配时睡眠时，无线网卡没有完全断电，导致唤醒后出现问题。在节能选择项取消勾选唤醒以供网络访问。
注：这个总是在10.13.X中不会出现。

### 2. 插入有线网卡时（以太网）出现自分配的IP地址
具体症状：
- 开机后第一次插入正常使用
- 重新拔插出现自分配的IP地址
- 睡眠唤醒后使用以太网出现自分配的IP地址
原因：有线网卡驱动问题
解决方法：使用命令重新载入网卡驱动，需要将网卡安装在/Library/Extension下
sudo kextunload /System/Library/RealtekRT8100.kext
sudo kextload /System/Library/RealtekRT8100.kext
### 3. 解决开机第二屏花屏
Clover中注入EDID,教程：http://bbs.pcbeta.com/viewthread-1762479-1-1.html

### 4. 白果三码（不能登录imessage和facetime的问题）
按照网上的无白果三码激活iMessage和Facetime时如果出现请联系客服代表。那就需要从身边有Mac电脑的同学或朋友提取。如果身边的朋友有Mac但不使用Mac系统，可以将[OSX PE](https://www.firewolf.science/firewolf-os-x-pe-v7-cn/)安装到U盘，然后运行iMessage Debug提取。

### 5. HDMI扩展屏幕显示模糊
https://www.jianshu.com/p/6274253b78d8

### 6. NTFS硬盘只能读取不能写入
- Tuxera NTFS(网上有破解版)

## 无解
1. 独立显卡（使用optimus技术的显卡无法驱动）
2. 读卡器（能显示为USB2.0-CRW）
3. PowerNap

## 暂时无解
1. 耳机麦克风输入
2. 10.13.4无法使用Twomon扩展屏幕软件


