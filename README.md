# Hackintosh EFI for HP OMEN 2Pro
## Configuration 配置信息
> 主板 : HP 8259 ( BIOS Version F.52 ) ( 不管你是i5还是i7都可以用 )  
    CPU : i5 7300HQ  
    内存 : 8Gx2 DDR4 2400MHz  
    SSD : 西数黑盘NVMe二代 SN700 250G ( NVMe默认开启TRIM )  
    HDD : HGST 1T 7200r  
    显卡 : Intel HD Graphics 630 + NVIDIA GeForce GTX 1050Ti ( 已通过WEG引导参数屏蔽 )  
    网卡 : Realtek RTL8111H + Dell Wireless 1820A ( BCM94350ZAE )  ( 替换原装的Intel Wireless-AC 7265，但使用并不稳定 )

## Progress 进展
* 引导方式
> CLOVER 已支持，测试版本 5103  
    OpenCore 已支持，测试版本 0.5.4
* 系统支持
> 理论支持macOS High Sierra 10.13.6 - Catalina 10.15之间的全部版本  
    建议安装macOS Mojave 10.14.6或更高版本，目前最高测试系统 Catalina 10.15.3 正式版  (19D76)
* 电源管理
> CPU变频：HWP+X86原生电源管理  
    睡眠和唤醒：支持合盖睡眠和唤醒，支持USB设备唤醒，CLOVER下可能出现RTC唤醒问题但影响不大，OC已无此问题，长时间睡眠后USB唤醒将失效（应该是进入深度睡眠了）  
    电池显示：电量百分比可显示，充放电正常，显示百分比多于实际值是由于X86变频导致的，如果出现百分比不变的情况请断开电源适配器关机长按电源键15S左右后先进win再进mac基本恢复正常
* 核显 HD630（0x591B8086）
> 支持亮度调节和保存（为确保调节后的亮度能通过原生NVRAM保存，请删除UEFI下的EmuVariableUEFI-64.efi，OC不需要这一步）  
    支持FN快捷键调节亮度  
    支持完整的H.264和HEVC解码
* 硬盘
>  添加Intel 10 Series AHCI识别  
    添加SATA SSD的TRIM开启补丁
* USB
> 已经正确定制所有端口，USB3.0正常，端口显示最大速率 5 Gbps  
    摄像头（HS04）可用，Intel蓝牙（HS07）半残
* 网卡
> 有线网卡：Realtek RTL8111千兆网卡，正常工作
* 声卡
> 已重新定制ALC295的布局文件，基于黑果小兵ID=13的修改  
    外放和耳机自动切换，麦克风需要手动切换（个人认为麦克风没必要自动切换）
* 触摸板
> 同步VoodooPS2的最新代码自编译驱动，支持原生手势，目前驱动可能存在抽风现象，是正常的，等开发团队逐步修复
    四指张合手势不可用，详见[VoodooPS2](https://github.com/acidanthera/VoodooPS2)项目的说明

## Instruction 使用说明
* 由于acidanthera的团队宣布他们后续版本的Lilu、VirtualSMC等相关内核扩展将不在CLOVER上进行测试，因此本项目将在不久后停止CLOVER部分的相关更新（除非有重要修复）
* 请确保你的BIOS为最新版本，当前版本为F.52，并关闭BIOS中的安全启动，如果需要双系统无缝切换，建议将OpenCore设置为第一引导
* 通过修复ACPI错误，OpenCore现在已经可以成功引导Windows 10了，注意事项如下：
    * 需要自己手动根据电脑的硬件UUID修改配置文件中SmUUID，否则很可能影响你的Windows 10激活情况以及电脑上众多软件的激活情况
    * 通过OpenCore引导的Windows系统将被识别为你SMBIOS所选的苹果机型，可能造成系统运行不稳定的情况请自行调试
* 请不要随意更改SMBIOS，因为这会影响到机型的识别，核显的驱动以及USB的定制，除非你自己会修改这些东西
* 为了能更好地获得macOS的体验，请自行查找关于白果三码的教程，用于激活macOS下iMessage和FaceTime

## Issues 存在的问题
* 独显 GTX1050/1050Ti
> Webdriver无法驱动，无解，因此HDMI外接也无解，无需多解释
* 无线网卡
> Intel 7265AC尚有大佬正在开发驱动但功能不完善  
    DW1820A（BCM94350ZAE）目前蓝牙存在冷启动无法上传固件问题，建议选择更换DW1560（BCM94352Z）
* 读卡器
> PCIe类型的读卡器不太好弄，已屏蔽，需要的话自己删除屏蔽文件和config的相关配置
* 由于暗影精灵的BIOS问题，电池可能在长时间睡眠或反复充放电后出现短暂的异常，常表现为百分比不发生变化，如果是第一次进，请务必断电长按电源键15S并在开机后先进win再进mac，之后基本就正常了

## 如果你有发现任何新问题，请提交issue，我将跟进修复
* 感谢 黑果小兵、Xjn、宪武等提供的教程和技术支持
* 感谢 acidanthera 的团队为黑苹果技术做出的贡献
