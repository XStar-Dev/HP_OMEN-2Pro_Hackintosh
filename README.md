# HP_OMEN-2Pro_Hackintosh
## Configuration 配置信息
* BIOS : F.52
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
    * OpenCore 已支持，测试版本 0.5.3
* 系统支持
    * 理论支持macOS High Sierra 10.13.6 - Catalina 10.15之间的全部版本
    * 建议安装macOS Mojave 10.14.6或更高版本，目前最高测试系统 Catalina 10.15.3 Beta版 (19D49f)

## Status 状态
* 电源管理
    * CPU变频：HWP+X86原生电源管理
    * 睡眠和唤醒：支持合盖睡眠和唤醒，关掉设置中的电能小憩可以解决RTC唤醒问题，现在已经支持USB唤醒了！
    * 电池显示：电量百分比可显示，充放电正常，显示百分比比实际值多是由于X86变频导致的
* 核显 HD630（0x591B8086）
    * 支持亮度调节和保存（为确保调节后的亮度能通过原生NVRAM保存，请删除UEFI下的EmuVariableUEFI-64.efi，OC不需要这一步）
    * 支持FN快捷键调节亮度
    * 支持完整的H.264和HEVC解码
* 硬盘
    * 添加Intel 10 Series AHCI识别
    * 添加SATA SSD的TRIM开启补丁
* 触摸板
    * 同步VoodooPS2的最新代码自编译驱动，支持原生手势
    * 四指张合手势不可用，详见[VoodooPS2](https://github.com/acidanthera/VoodooPS2)项目的说明
* USB
    * 已经正确定制所有端口，USB3.0正常，端口显示最大速率5 Gbps
    * 摄像头（HS04）可用，Intel蓝牙（HS07）半残
* 网卡
    * 有线网卡：Realtek RTL8111千兆网卡，正常工作
* 声卡
    * 已重新定制ALC295的布局文件，基于黑果小兵ID=13的修改
    * 外放和耳机自动切换，麦克风需要手动切换（个人认为麦克风没必要自动切换）

## What's not working? 哪些不能工作？
* 独显 GTX1050/1050Ti
    * Webdriver无法驱动，无解，因此HDMI外接也无解，无需多解释
* 无线网卡
    * Intel 7265AC尚有大佬开发但功能不完善，由于DW1820A（BCM94350ZAE）目前蓝牙存在问题，建议选择更换DW1560（BCM94352Z）
* 读卡器
    * PCIe接口的读卡器无解，即便是USB的也不容易搞

## Issues 存在的问题
* 如果反复进行放电再充电到满电（一般人不会这么做），最后可能出现电量锁在100%不再变化，此为暗影精灵通病，原因来自ACPI中的代码，尚未找到完美的解决方案
* 如果你使用的是DW1820A无线网卡，蓝牙部分可能在冷启动mac时无法上传固件而导致开机卡死或者直接崩溃，目前仍未找到合适的解决方案
* 睡眠唤醒后触摸板可能漂移失灵，等待VoodooPS2的作者修复

## Advanced 后期进展
* 由于acidanthera的团队宣布以后他们的Lilu、VirtualSMC等相关内核扩展将不在新的CLOVER上进行测试，CLOVER相关的测试和升级将基本停止
* 通过OpenCore引导Windows目前仍存在问题，请从主板BIOS F9菜单进入Windows系统，后续解决后将尽快更新OC的EFI

## 如果你有发现任何新问题，请提交issue，我将跟进修复

## 感谢
黑果小兵、Xjn、宪武等
