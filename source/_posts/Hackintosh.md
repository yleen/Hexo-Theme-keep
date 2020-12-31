---
title: Hackintosh #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2020-12-11 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: #文章分类目录，可以为空，注意:后面有个空格
tags: [hackintosh,efi,系统] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
description: 黑苹果配置 #概要信息
---



macintosh 麦金塔

# Issue

- 黑苹果 屏幕亮度

- win下关机变重启

  打开 config.plist 文件并勾选 Acpi 部分的 FixShutdown 选项解决的

- 随航问题  目前高度怀疑是核显没激活

BIOS 中设置主显卡为独显 (PCIE) 并且设置 DVMT 为 128M，以便独显、核显并存

enabling "Above 4G Decoding."

bios设置主显卡为核显才能启动随航。

- 引导界面多余选项

https://blog.daliansky.net/OpenCore-BootLoader.html  find: Boot: 引导界面的设置

https://www.mfpud.com/topics/2984/   删除macOS黑苹果系统四叶草引导Clover启动界面的多余启动硬盘项

misc-security-scan policy 改为983299   Hideself改成TRUE

# 安装环境

- 配置
```
cpu: i7 8700 coffee lake
graphics: MAXSUN RX580 2304 Polaris
motherboard: Asus Z390-A
ram: Micron 8g X2
rom: WD sn550 500g X2
```
- BIOS 设置

`禁用`

| 英文 | 中文 |
| --- | --- |
| Fast Boot | 快速启动 |
| CFG Lock (MSR 0xE2 write protection) | CFG 锁 (MSR 0xE2 写入保护) |
| VT-d | [VT-d](https://zhidao.baidu.com/question/495526512.html) |
| CSM  | 兼容性支持模块 |
| Intel SGX | Intel SGX |

`启用`

| 英文 | 中文 |
| --- | --- |
| VT-x | [VT-x](https://zhidao.baidu.com/question/495526512.html) |
| Above 4G decoding | 大于 4G 地址空间解码 |
| Hyper Threading | 处理器超线程 |
| Execute Disable Bit | 执行禁止位 |
| EHCI/XHCI Hand-off | 接手 EHCI/XHCI 控制 |
| OS type: Other | 操作系统类型: Other |
| Legacy RTC Device | 传统 RTC 设备 |
| restore on ac power loss | 通电自启 |
打开 Ahci 选项

# 引导介绍

## EFI 文件结构

打开下载好的最新版 OC，把 Doc 文件夹下面的 Sample.plist 改名为 config.plist，并把此文件移动到 EFI 目录下面。

`EFI-OC-Kexts`

打开 Kexts，我们把常用的一些 kexts 先放进去，一般情况下你需要放如下 Kexts:

| 文件名 | 解析 |
| --- | --- |
| Lilu.kext | Acidanthera驱动全家桶的SDK |
| Applealc.kext | 声卡驱动 |
| VirtualSMC.kext | 传感器驱动依赖 |
| SMCProcessor.kext | CPU核传感器 |
| SMCSuperIO.kext | IO传感器 |
| WhateverGreen.kext | 显卡驱动 |
| IntelMausi.kext | Intel类千兆网卡驱动 |
| Usbinjectall.kext | USB驱动（你也可以定制自己的USB补丁）|
| NVMeFix.kext | 为NVME硬盘增加ASPT属性来保证节电，虽然对台式机没啥用，但是官方推荐所有NVME用户都使用此补丁 |

`EFI-OC-Drives`

打开Drives,里面的驱动介绍如下：

| 文件名 | 解析 |
| --- | --- |
| HfsPlus.efi | 用于HFS格式文件系统，这是必须加载的。 |
| OpenRuntime.efi | 内存运用等必要的插件，必须加载。 |
| MemoryAllocation.efi | 为 Z390/X99 等主板预留第一组 512MB 内存, 帮助引导工具注入内核以及内核缓存至第一组 512MB 内存, 需要配合 FwRuntimeServices 和引导标识符 `slide=1` [参考](https://blog.daliansky.net/OpenCore-BootLoader.html) |
| OpenCanopy.efi | 加载第三方开机主题。(目前没有主题) |

这个安装阶段中,要确保是从硬盘启动而不是 u盘. 从 u 盘启动将会重新开始最初的安装过程.

注：如果用的第三方制作的镜像，EFI分区可能已经有了EFI文件夹，请删除EFI分区内的所有文件后将你的EFI文件夹拖入到EFI分区

they released ACPI patch for our hacks to get native NVRAM (like older motherboards, only difference is in that we need additional .aml file in our EFIs). 


U盘写好系统后,打开DiskGenius,把自己型号的clover复制到U盘的EFI文件夹替换原clover[^1]

- 官方支持的其他文件

***`drivers`*文件夹下**

`AppleUsbKbDxe.efi`

>这个驱动是给使用模拟 UEFI 的老主板在 OpenCore 界面正常输入用的, 请勿在 Ivy Bridge (3 代酷睿)及以上的主板上使用 (详见 vit9696 的解释)

`NvmExpressDxe.efi`

>用于在 Haswell (4 代酷睿) 或更老的主板上支持 NVMe 硬盘, 新主板不需要

`XhciDxe.efi`

>用于给 Sandy Bridge (2 代酷睿) 或更老的主板上支持 XHCI, 新主板不需要

`HiiDatabase.efi`

>用于给 Ivy Bridge (3 代酷睿) 或更老代主板上支持 UEFI 字体渲染, UEFI Shell 中文字渲染异常时使用, 新主板不需要

`AudioDxe.efi`  

> 开机UEFI界面若需要声音效果需要加载。

`CrScreenshotDxe.efi `

> 开机UEFI的截图工具。

`OpenUsbKbDxe.efi `

> 给使用模拟 UEFI 的老主板在 OpenCore 界面正常输入用的, 请勿在 Ivy Bridge (3 代酷睿)及以上的主板上使用。

`Ps2KeyboardDxe.efi`

> PS2键盘所需插件。

`Ps2MouseDxe.efi `

> PS2鼠标所需插件。

`UsbMouseDxe.efi `

> 当MacOS被安装在虚拟机上所需要的鼠标插件。

`ACPIBatteryManager.kext`

> 电源管理驱动

***`tools`*** **文件夹下**

`BootKicker.efi`

>调用苹果原生的引导切换 GUI, 黑苹果不支持

`CleanNvram.efi`

>OpenCore 自带的 NVRAM 清理功能已经足够我们使用

`GopStop.efi`

>停止显卡 GOP, 排错时使用

`HdaCodecDump.efi`

>导出声卡 Codec, 可用于定制声卡, 需要时可以临时加回来

`VerifyMsrE2.efi`

>用于检查主板上 CFG 锁的状态

![EFI文件结构](http://7.daliansky.net/OpenCore/Structure.png)

现在, 我们可以把 AppleSupportPkg 中必需的 .efi 驱动程序放入 Drivers 文件夹, 将 你的 kext 和 DSDT/SSDT 放入各自的文件夹中。请注意, OpenCore 不支持支持列表以外的 UEFI 驱动程序!

## config.plist介绍

**PollAppleHotKeys:** `YES`

设置为 YES 后允许在**引导过程**中使用苹果原生快捷键, 需要与 Quirk KeySupport=Yes 结合使用, 具体体验取决于主板固件。

快捷键组合:

```
Cmd + V: 启用 -v 跑码
Cmd + Opt + P + R: 重置 NVRAM
Cmd + R: 启动恢复分区
Cmd + S: 启动至单用户模式
Option / ALT: 在 ShowPicker 设置成 NO 时显示引导项选择界面, ALT 不可用时可用 ESC 键代替
Cmd + C + 减号: 关闭主板兼容性检查, 等同于添加引导标识符 -no_compat_check
Shift: 安全模式
```

# 安装原理

## 使用的软件
 clover 引导启动系统
 opencore 引导启动系统


## 系统引导原理
1. 白苹果： 电脑加电 → 启动 Mac 的 UEFI Bios → 加载 NVRAM → 进入 macOS
2. clover: 电脑加电 → 加载四叶草的 EFI 微系统 → 模拟白果的 EFI 和加载参数 → 进入 macOS
3. Ozmosis：电脑加电 → 启动 Ozmosis 模拟的 Mac 的 Uefi Bios → 加载 NVRAM → 进入 macOS

Clover的引导方式是通过用户创建EFI分区来引导仿冒的设备信息，从而实现系统的安装，但是由于Clover是通过仿冒白苹果机型来完成安装，所以必须要求所安装的黑苹果硬件和白苹果相似。

opencore引导是clover的升级版，它相较于Clover的最大优势就是，它在mac升级系统的时候只需要升级kexts驱动文件即可，不必要升级OC引导本身

Oz（[Ozmosis](http://imacosx.com/scb/1886.html)）引导，通过将BIOS刷成和白果一样达到完美效果，不过这种方法兼容性差，容易把主板刷报废，目前已经不更新了。

# 收集的一些~~坑~~经验

分区文件DiskGenius  下一个稳定版本的 不然很容易报错
TransMac 这个软件 下一个绿色版的 试用版的容易出错  我就是出错了 然后就挂了 又一次下载镜像   建议使用 balenaEtcher

>Every motherboard has NVRAM, but Intel 300 series (except z370) like B360, Z390, H370, etc. has other implementation of it that previous motherboards and that’s why we needed emulated nvram for nice hackintoshy stuff.
they released ACPI patch for our hacks to get native NVRAM (like older motherboards, only difference is in that we need additional .aml file in our EFIs). (https://www.reddit.com/r/hackintosh/comments/ercqom/native_nvram_on_z390_possible/)

所有AMD免驱显卡均建议搭配Lilu.kext和WhateverGreen.kext使用，以避免启动黑屏、流处理器被部分禁用、显存不识别等毛病，其它原因同NVIDIA显卡；

AMD 显卡在黑苹果中免驱，指的是只要把这个显卡插上，搭配 WhateverGreen.kext （下称“WEG”）的情况下 macOS 就能识别出对应的型号，并提供基础性能驱动。其实这个时候，WEG 仅仅只是调用了针对某一核心架构的通用驱动，例如 RX470/RX570/RX580 核心都是 Polaris，RX5500/RX5600/RX5700XT 核心都是 Navi，WEG 分别调用了针对某一核心的通用驱动，实现了基本驱动和基本性能。

但是。这里要说但是了。

这样的驱动并不能发挥显卡最大的性能，举几个实际案例：在 Geekbench 的性能测试中，Radeon VII 的得分仅为 5-6 万分，而在 Windows 上可以轻松达到 8.2 万分以上；Radeon RX 5700XT 的 Metal 性能得分大概是 3.8 万分左右，而在解决这个问题后能轻松跑到 6.4 万分以上（macOS 10.15.4 以后能达到 7 万分以上），并且在超频后差距更大。
_称为 SMU 的固件（SMU Firmware）_  针对这个情况，可以使用由 @CMMChris 开发的第三方驱动 RadeonBoost.kext
如果使用后黑屏，请添加 adgpmod=pikera 这个启动参数

参考：

[2020年黑苹果macOS Catalina显卡支持列表，持续更新中](https://heipg.cn/tutorial/graphic-card-for-macos-2020.html)
[修复SMU固件引起的显卡问题](https://heipg.cn/tutorial/fix-smu-firmware-for-radeon.html)


## 常见问题及解决方案
### MacOS 与 Windows 时间不一致
Windows 管理员运行cmd 输入命令：Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1

原因：Windows 与Mac计算时间方式不同，Windows直接读取UTF时间，Mac自动UTF+8



### 开机显示American Megatrends

<img src="https://i.loli.net/2020/12/31/QJc7kYuSb6tqNnX.png" alt="image.png" style="zoom:50%;" />

config.plist->NVRAM->Add->7C436110-AB2A-4BBB-A880-FE41995C9F82->boot-args
添加字段  rtcfx_excloud=80-FF  从00-FF 自己试

### Open Core 默认选项无法修改

Apple -> System Preferences -> Startup Disk  设置MacOS 启动盘为启动项
在 Open Core 中选择你想设为默认的系统 Ctrl+Enter 即可

### bios 黑屏 可正常进系统

clean nvram 即可
```
Hi, i'm back. I cleaned the NVRAM, after that I accidentally pushed the "del" key and the BIOS page has appeared.

Im theory yes CMD + ALT + P + R, but I use OpenCore as boot loader and there is an option for that.

You can try using terminal:

sudo nvram -c

sudo shutdown -r now

Restart the PC and try to access BIOS.

Let me know.
```

### 音频输出

Config–DeviceProperties

此项是用来注入你的设备的，主要是显卡和声卡两部分。同样你也可以定制一些设备到你的 `关于本机--系统报告--PCI` 列表中，尽管没有多大的意义。

声卡

这里首先我们需要确认自己的声卡驱动已经被加载，终端下输入：

```bash
kextstat | grep -E "AppleHDA|AppleALC|Lilu"
```

我们会得到被加载的驱动，请确保 `as.vit9696.Lilu`；`as.vit9696.AppleALC`；`com.apple.driver.AppleHDAController`；`com.apple.driver.AppleHDA` 已经被加载。

找自己声卡的地址，准备好在文章开头要求下载的 `gfxutil`，将 `gfxutil` 程序放在桌面，输入:

```bash
~/desktop/gfxutil -f HDEF //一般来说我们在使用 Applealc 后，板载声卡的部件名都叫 HDEF
```

我们输入后会得到声卡的PCI路径，比如我输出的就是：

```bash
00:1f.3 8086:a2f0 /PC00@0/HDEF@1F,3 = PciRoot(0x0)/Pci(0x1F,0x3)
```

这里我们找到的声卡 PCI 路径为 `PciRoot(0x0)/Pci(0x1f,0x3)`。我们把预先填写在那里的 `PciRoot(0x0)/Pci(0x1b,0x0)` 项替换成我们真正的声卡路径。

后面一段我们看到预先填写的声卡ID为 `<01000000>`，这里我们需要把它换成合适自己声卡的 ID，输入以下命令得到自己声卡的 CodecID。

```bash
ioreg -l|grep IOHDACodecVendorID
```

点击[此页面](https://github.com/headkaze/Hackintool/blob/master/Resources/Audio/Codecs.plist)搜索刚得到的CodecID就可查询到自己声卡的型号名称，以及可用的 `LayoutID`。

比如我的 CodecID 为 `283906408`，声卡型号 `ALCS1220A`，对应 1, 2, 3, 5, 7, 11, 13, 15, 16, 27, 28, 29, 34 的 `layout ID`。我们需要一个个测试过去，选定自己能用的。这里我们选择 7 这个 ID 进行测试，将 7 转化成 16 进制格式为 07，后面为了满足格式要求添加 6 个 0，则为 `07000000`，将这个值替换刚才预先填的` 01000000` 中；如果我们测试 ID 为 27，27 的 16 进制为 1b，补上 6 个 0 则为 `1b000000`。

```
PciRoot(0x0)/Pci(0x1f,0x3)
        device-id       data      <70a10000> //一般情况下这段是不需要填写的，除非你的声卡需要仿冒
        layout-id       data      <0b000000> //这个Layout id我瞎写的，你按实际情况写
```

如果你测试的ID都无效，请确保你的操作过程正确、并测试用万能声卡(VoodooHDA)补丁来替换 AppleALC 这个补丁。如果都不行，你可能需要自行[编译声卡补丁](https://blog.daliansky.net/Use-AppleALC-sound-card-to-drive-the-correct-posture-of-AppleHDA.html)。

参考：[2.3 Config–DeviceProperties](https://blog.xjn819.com/post/opencore-guide.html)













## Mac与Win的差异
### 引导方式
- **Windows**

主要的系统引导方式有两种：传统的Legacy BIOS 和新型的 UEFI BIOS

***Legacy BIOS***

无法识别GPT分区表格式

***UEFI BIOS***

UEFI是一个微型操作系统，放在固件中，能识别FAT文件系统，运行efi程序

可同时识别MBR分区和GPT分区，都可用于启动操作系统。

> Boot Loader：
>
> 对于PC平台来说Boot Loader=BIOS+启动管理器+启动文件+载入程序（IPL+SPL），但是一般指启动管理器及启动设置文件。
> 功能：启动管理器+启动设置文件：提供启动选项，定位、加载并执行SPL。
> SPL：加载OS内核，SPL只认识自己分区的内核文件。

***Legacy BIOS+MBR引导原理***

引导过程：上电–>Legacy BIOS–>MBR–>DPT–>PBR–> Bootmgr（vista开始）/NTLDR–>BCD（vista开始）/boot.ini–>Winload.exe–>内核加载 –>windows vista/windows xp

***UEFI BIOS+GPT 引导原理***

引导过程：上电–>UEFI–>GPT分区表–>EFI分区–>\efi\Microsoft\boot\bootmgfw.efi–>efi\Microsoft\BCD→\Windows\system32\winload.efi。

- **MacOS**



参考：
[windows引导过程以及多系统引导原理](https://blog.csdn.net/liao20081228/article/details/82591728)
[初步了解计算机与操作系统启动原理](https://www.jianshu.com/p/26e184605952)





[^1]:???遗留笔记  应该没啥大用