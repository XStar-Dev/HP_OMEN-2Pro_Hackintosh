# HP_OMEN-2Pro_Hackintosh
Hackintosh EFI For HP OMEN 2Pro

# What's working?
### 系统支持
    理论支持macOS High Sierra 10.13.6 - Catalina 10.15之间的全部版本
    建议安装macOS Mojave 10.14.6或更高版本
### 电源管理
    CPU变频：X86 + HWP
    睡眠和唤醒：支持合盖睡眠和唤醒
    电池显示：电量百分比可显示，充放电正常，但如果反复进行放电再充电到满电（一般人不会这么做），最后可能出现电量锁在100%不再变化，暗影精灵通病
### 硬盘识别
    添加Intel 10 Series AHCI识别
    添加SATA SSD的TRIM开启补丁
### USB
    已经正确定制所有端口，USB3.0正常，端口最大速率5.0GB/s
    摄像头（HS04）可用,Intel蓝牙（HS07）半残
### 核显 HD630（0x591B8086）
    支持亮度调节和保存，支持FN快捷键调节（为确保调节后的亮度能被保存，请删除drivers64UEFI下的EmuVariableUEFI-64.efi）
    支持完整的H.264和HEVC解码
### 网卡
    有线网卡：RTL8111千兆网卡，正常工作
### 声卡
    ALC295已重新定制，基于黑果小兵ID=13的修改
    外放和耳机自动切换，麦克风需要手动切换（个人认为麦克风没必要自动切换）
### 触摸板
    基于VoodooPS2的最新代码自编译驱动
    支持原生手势，其中四指手势不可用，详见VoodooPS2项目的说明
https://github.com/acidanthera/VoodooPS2

# What's not working?
### 独显 GTX1050/1050Ti
    Webdriver无法驱动，无解，因此HDMI也无解，无需多解释
### 无线网卡
    Intel无线网卡无解，建议更换BCM94352Z，我本人使用的BCM94350ZAE（DW1820A）不完美，冷启动异常，尚未解决

# Next Step
### OpenCore
    新的引导方式OpenCore正在逐渐发展和完善，接下来将使用OpenCore进行配置和调试，CLOVER引导如无大问题将暂时不再更新
