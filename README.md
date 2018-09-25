## 硬件清单

| 硬件         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| CPU          | i5-5200U(Broadwell)                                          |
| 主板芯片组   | 9CC3笔记本芯片组（Intel 9系）                                |
| 显示屏       | LG 1080P（官方是720P屏幕，换了1080P的屏幕）                  |
| **核显**     | **HD5500**                                                   |
| 独显         | Nvidia 820M（Optimus技术的显卡无法驱动）                     |
| 硬盘         | 250G 三星850EVO SSD + 500G 机械硬盘（拆光驱）                |
| **声卡**     | **ALC255**+ HDMI音频（二合一接口耳麦暂时无解）               |
| 有线网卡     | Realtek 8106E                                                |
| **无线网卡** | **BCM94352HMB**（官方的是AR9565，不支持5G频段）              |
| 蓝牙         | 20702A3（BCM94352HMB自带蓝牙4.0，支持Handoff）               |
| 读卡器       | Realtek USB2.0 Card-Reader（可在虚拟机中驱动，推荐Parallels的Win10融合模式） |
| USB接口      | 一个USB3.0+两个USB2.0                                        |
| 摄像头       | 普通集成摄像头                                               |
| 触控板       | Symthetic                                                    |

## 相关资源请到我的[Github](https://github.com/kkzzhizhou/Dell-3443-Hackintosh)仓库下载

## Clover版本：4658

## MacOS版本：Mojave 10.14.1

## ACPI/patched目录说明

| 文件                 | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| DSDT.aml             | 已打补丁如下：<br />- [igpu] Brightness fix（亮度调节补丁） <br />- Fix ADBG Error(解决编译报错) <br />- Fix PARSEOP_ZERO Error(aggressive)(解决编译报错) <br />- IRQ Fix(修复声卡找不到的问题)<br />- USB3_PRW 0x0D(instand wake)(修复睡眠问题) |
| SSDT-HDMI-HD5500.aml | 开启HD5500 HDMI音频                                          |
| SSDT-UIAC-ALL.aml    | Dell-3443 USB接口定制                                        |
| ssdt.aml             | CPU变频文件，用于加载X86PlatformPlugin.kext和X86PlatformShim.kext（有利于睡眠） |

## kexts（驱动）目录说明

| 文件                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| SMCBatteryManager.kext      | 电源驱动，用于正常显示电量                                   |
| AirportBrcmFixup.kext       | 无线网卡驱动，在config.plist启动参数中添加brcmfx-country=#a -brcmfxbeta -lilubeta才能正常工作 |
| ApplePS2SmartTouchPad.kext  | 键盘，触摸板和鼠标驱动，自己对驱动进行的定制，更接近白苹果的操作体验 |
| BrcmFirmwareData.kext       | 配合BrcmPatchRAM2.kext驱动蓝牙                               |
| BrcmPatchRAM2.kext          | 如果卡在Fail to write to pulk pipe，需要添加启动参数dart=0以及Drop掉DMAR表，这个与VT-d虚拟技术有关。 |
| FaceTime_WebCam.kext        | 仿冒Facetime摄像头驱动                                       |
| VirtualSMC.kext             | 黑苹果必备                                                   |
| Lilu.kext                   | 黑苹果必备                                                   |
| WhateverGreen.kext          | 核显驱动，已在config.plist勾选Inject Intel并注入Platform-ID为：16260006 |
| IntelGraphicsDVMTFixup.kext | 核显补丁驱动，解决某些笔记本DVMT内存分配问题                 |
| RealtekRT8100.kext          | 有线网卡8106E驱动                                            |
| AppleALC.kext               | 声卡驱动，已在Config.plist中注入ID:3                         |
| USBInjectAll.kext           | 配合SSDT-UIAC-ALL.aml定制USB                                 |

## Drivers64UEFI目录说明
| 文件                  | 作用                                               |
| --------------------- | -------------------------------------------------- |
| APFS.efi              | 苹果新推出的`apfs`文件系统，macOS 10.13/10.14必备  |
| OsxAptioFixDrv-64.efi | 修复AMI Aptio EFI内存映射，一般用于解决卡+号的问题 |
| FSInject.efi          | 控制文件系统注入kext到系统的可能性                 |
| VBoxHfs-64.efi        | HFS+文件系统驱动                                   |

## FAQ

1. **睡眠唤醒后，网卡延迟增高，需要断开重连电源适配器才能正常工作**

   **原因：**当插着电源适配时睡眠时，无线网卡没有完全断电，导致唤醒后出现问题

   **解决方法：**在节能选择项取消勾选唤醒以供网络访问。

   **系统版本：**10.12.X

2. **插入有线网卡时出现自分配的IP地址**

   **具体症状：**

   - 第一次插入正常使用
   - 重新拔插出现自分配的IP地址
   - 睡眠唤醒后使用以太网出现自分配的IP地址

   **原因：**有线网卡驱动问题

   **解决方法：**使用命令重新载入网卡驱动，需要将网卡安装在/Library/Extension下
   sudo kextunload /System/Library/RealtekRT8100.kext
   sudo kextload /System/Library/RealtekRT8100.kext

3. **不能登录imessage和facetime的问题**

具体症状：激活iMessage和Facetime时如果出现请联系客服代表。

解决方法：可以考虑从身边有Mac电脑的同学或朋友提取，如果身边的朋友有Mac但不使用Mac系统，可以将[OSX PE](https://www.firewolf.science/firewolf-os-x-pe-v7-cn/)安装到U盘，然后运行iMessage Debug提取。

4. **HDMI扩展屏幕显示模糊**

   原因：Macbook 外接显示器默认为 TV 模式，TV 渲染模式下，文字效果非常非常非常差

   解决方法：https://www.jianshu.com/p/6274253b78d8

5. **NTFS硬盘只能读取不能写入**

   原因：MacOS原生不支持NTFS写入操作

   解决方法：使用第三方工具，推荐Tuxera NTFS

6. **如何彻底删除Clover中启动项：FileVault和Prebooter**

   在终端中使用命令：

   ```bash 
   # 查看硬盘分区
   diskutil list
   # 找到并挂载Prebooter所在的分区号，比如disk1s1
   diskutil mount disk1s1
   # 进入/Volume/Prebooter，删除目录下的文件夹即可
   ```

7. **使用AppleALC后左右声道平衡出现问题？**

   ```bash
   # 下载最新的CodecCommander，将其中的hda-verb移动到/usr/bin/目录下
   sudo mv ./hda-verb /usr/bin
   # 之后用KCPM Utility Pro安装CodecCommander到/Library/Extension/目录并重建缓存
   
   ```

8. **如何彻底删除Clover中多余的启动项：FileVault和Prebooter**

   在终端中使用命令：

   ```bash
   # 查看硬盘分区
   diskutil list
   # 找到并挂载Prebooter所在的分区号，比如disk1s1
   diskutil mount disk1s1
   # 进入/Volume/Prebooter，删除目录下的文件夹即可
   ```




## 帮助

- [远景论坛](http://bbs.pcbeta.com/)
- [tonymacx86](http://www.tonymacx86.com/)


