# HP_OMEN-2Pro_Hackintosh
## Configuration 配置信息
* 主板 : HP 8259（BIOS Version F.52 ）
* CPU : i5 7300HQ
* 内存 : 8Gx2 DDR4 2400MHz
* SSD : 西数黑盘NVMe二代 SN700 250G（NVMe默认开启TRIM）
* HDD : HGST 1T 7200r
* 显卡 : Intel HD Graphics 630 + NVIDIA GeForce GTX 1050Ti（已通过WEG引导参数屏蔽）
* 网卡 : Realtek RTL8111H + Dell Wireless 1820A (BCM94350ZAE) (替换原装的Intel Wireless-AC 7265，但使用并不稳定)
* 备注 : 如果你的设备是i7或者GTX1050的也是可以用的，这并不影响启动，因为它们是通用的
    
## Progress 进展
* 引导方式
    * CLOVER 已支持，测试版本 5103
    * OpenCore 已支持，测试版本 0.5.4
* 系统支持
    * 理论支持macOS High Sierra 10.13.6 - Catalina 10.15之间的全部版本
    * 建议安装macOS Mojave 10.14.6或更高版本，目前最高测试系统 Catalina 10.15.3 正式版  (19D76)
    * 通过修复ACPI错误，OpenCore现在已经可以成功引导Windows 10了，注意事项如下：
        * 需要自己手动根据电脑的UUID修改配置文件，否则很可能影响你的Windows 10激活情况以及电脑上众多软件的激活情况，具体教程后续更新
        * 通过OpenCore引导的Windows系统将被识别为你SMBIOS所选的苹果机型，可能造成系统运行不稳定的情况请自行调试
    * 为了能更好地兼顾macOS与Windows 10，请自行查找关于白果三码的教程，并正确填写主板UUID和系统UUID，以保持Windows的激活状态和macOS下iMessage和FaceTime的激活状态

## Status 状态
* 电源管理
    * CPU变频：HWP+X86原生电源管理
    * 睡眠和唤醒：支持合盖睡眠和唤醒，支持USB设备唤醒，CLOVER下可能出现RTC唤醒问题但影响不大，OC已无此问题
    * 电池显示：电量百分比可显示，充放电正常，显示百分比多于实际值是由于X86变频导致的，如果出现百分比不变的情况请断开电源适配器关机长按电源键15S左右后先进win再进mac基本恢复正常
* 核显 HD630（0x591B8086）
    * 支持亮度调节和保存（为确保调节后的亮度能通过原生NVRAM保存，请删除UEFI下的EmuVariableUEFI-64.efi，OC不需要这一步）
    * 支持FN快捷键调节亮度
    * 支持完整的H.264和HEVC解码
* 硬盘
    * 添加Intel 10 Series AHCI识别
    * 添加SATA SSD的TRIM开启补丁
* USB
    * 已经正确定制所有端口，USB3.0正常，端口显示最大速率 5 Gbps
    * 摄像头（HS04）可用，Intel蓝牙（HS07）半残
* 网卡
    * 有线网卡：Realtek RTL8111千兆网卡，正常工作
* 声卡
    * 已重新定制ALC295的布局文件，基于黑果小兵ID=13的修改
    * 外放和耳机自动切换，麦克风需要手动切换（个人认为麦克风没必要自动切换）
* 触摸板
    * 同步VoodooPS2的最新代码自编译驱动，支持原生手势，目前驱动可能存在抽风现象，是正常的，等开发团队逐步修复
    * 四指张合手势不可用，详见[VoodooPS2](https://github.com/acidanthera/VoodooPS2)项目的说明

## What's not working? 哪些不能工作？
* 独显 GTX1050/1050Ti
    * Webdriver无法驱动，无解，因此HDMI外接也无解，无需多解释
* 无线网卡
    * Intel 7265AC尚有大佬正在开发驱动但功能不完善，由于DW1820A（BCM94350ZAE）目前蓝牙存在问题，建议选择更换DW1560（BCM94352Z）
* 读卡器
    * PCIe类型的读卡器不太好弄，已屏蔽

## Issues 存在的问题
* 如果反复进行放电再充电到满电（一般人不会这么做），最后可能出现电量锁在100%不再变化，此为暗影精灵通病，原因来自ACPI中的代码，尚未找到完美的解决方案
* 如果你使用的是DW1820A无线网卡，蓝牙部分可能在冷启动mac时无法上传固件而导致开机卡死或者直接崩溃，目前仍未找到合适的解决方案

## Advanced 后期进展
* 由于acidanthera的团队宣布以后他们的Lilu、VirtualSMC等相关内核扩展将不在新的CLOVER上进行测试，CLOVER相关的测试和升级将基本停止

## 如果你有发现任何新问题，请提交issue，我将跟进修复
* 感谢 黑果小兵、Xjn、宪武等提供的教程和技术支持
* 感谢 acidanthera 的团队为黑苹果技术做出的贡献
