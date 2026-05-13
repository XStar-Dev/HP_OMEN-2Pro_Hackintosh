# Hackintosh EFI for HP OMEN 2Pro (15-ax2xx Laptop Series)

## Configuration 配置信息

> 本人机型：OMEN by HP Laptop 15-ax225TX
> 海外高分辨率机型请参考网上的 HiDPI 教程定制
> 本 EFI 可能适用于同模具的 Pavilion 型号，具体请自行测试

| 硬件 | 型号 | 备注 |
| ----- | ------ | ------ |
| 主板 | HP 8259 ( BIOS Version F.56 ) | 2020.12 更新 |
| CPU | Intel Core i5 7300HQ 2.5GHz  | 800-3500MHz |
| 内存 | 8Gx2 DDR4 2400MHz | 额外加了一条 8G |
| SSD | 西数黑盘 NVMe 二代 SN700 250G | NVMe SSD 默认开启 TRIM |
| HDD | HGST 1T 7200r | \ |
| 显卡 | Intel HD Graphics 630 + NVIDIA GeForce GTX 1050Ti | 通过 SSDT 关闭独显 |
| 声卡 | Realtek ALC295  | A171 - CM238 HD Audio |
| 网卡 | Realtek RTL8111H + Intel Wireless 7265AC  | Wi-Fi和蓝牙均可正常使用 |

## 更新说明

- 更新 `OpenCore` 到 `1.0.7` 正式版
- 同步更新三大核心驱动 `Lilu` `VirtualSMC` `WhateverGreen`
- 同步更新最新的 `VoodooPS2` 键盘和触摸板驱动
- 同步最新的 `itlwm` `IntelBluetoothFIrmware` `IntelBTPatcher` Wi-Fi 和蓝牙驱动
- 由于 macOS 从 `Monterey` 开始修改了蓝牙驱动逻辑，本 EFI 已经针对不同 macOS 版本做了适配，在 `Monterey` 及以上的版本启用 `BlueToolFixup.kext`，在其他低版本仍然采用 `IntelBluetoothInjector.kext`，为了能正常使用蓝牙，请勿随意修改 `config.plist` 中的相关内容
- 引用或转发本 EFI 请务必注明出处，所有内容仅供个人使用和学习参考，严禁用于商业用途（过去停更的几年已经发现多家tb商家使用本 EFI 进行牟利，请各位黑苹果爱好者注意甄别）

## Progress 进展

- 引导方式
  - `OpenCore` 已支持，运行版本 `1.0.7`
- 系统支持
  - 理论支持 macOS High Sierra 10.13.6（17G65）- Ventura 13.7.8（22H730）之间的全部版本
  - 建议直接安装 macOS Mojave 10.14.6 或更高版本
- 电源管理
  - CPU变频：已启用 X86 原生电源管理，采用 `CPUFriend` 注入 `MBP14,3` 电源管理参数，将闲时频率从 1.2GHz 降低到 0.8GHz，并更改 `EPP` 为节能模式（0x80）
  - 睡眠和唤醒：支持合盖睡眠和唤醒，支持 USB 设备唤醒（当使用电池进行睡眠时，所有外置设备将自动断电无法进行唤醒）
  - 电池显示：电量百分比可显示，充放电正常，开启 X86 原生电源管理后状态栏百分比多于实际值，目前尚不明确具体原因，因此从 100% 满电开始放电时电量变化将会比较缓慢（即便实际值已经是 95% 状态栏仍然显示为 100%）
- 核显 HD630（0x591B8086）
  - 显示正常，支持完整的 `H.264` 和 `HEVC` 解码
  - 亮度调节：系统设置和键盘快捷键均可正常调节和保存
  - 如需使用 `HiDPI` 请参考网上的教程，使用脚本开启容易出现花屏问题（仅建议海外 4K 版本适用）
- 硬盘
  - 通过 Device Properties 注入 10 Series AHCI 识别信息
  - 通过 Device Properties 注入 Apple SSD Controller 识别信息
- USB
  - 已经正确定制所有端口，USB3.0正常，端口显示最大速率 5 Gbps（如果有时候插上不显示为 5 Gbps，属正常现象，因为 `Windows` 下同样会出现这种情况，是接口接触问题）
  - 内建设备：摄像头（HS04）可用，蓝牙（HS07）
- 触摸板
  - 基本支持所有手势，当然由于四指手势是模拟出来的且触摸板面积较小，所以并不灵敏
- 声卡
  - 注入黑果小兵 `ALC295` 的 `layout-id=13` ，由于外置麦克风存在切换和热启动无电平输入问题，故未显示，建议平常使用内置麦克风即可
- 网卡
  - 有线网卡：Realtek RTL8111 千兆网卡，正常工作（高负载下可能出现断连问题，请尝试断开网线后重新插入联网）
  - 无线网卡：Intel Wireless 7265AC
    - WiFi：采用 `itlwm` 驱动，可正常连接 Wi-Fi，速率可达 300Mps，应该未到最高，我的路由器有点不太行了
    - 蓝牙：采用 `IntelBluetoothFirmware` 上传固件程序至硬件模块，再通过 `IntelBTPatcher` 进行打补丁完成驱动，在 `Monterey` 及以上的版本启用 `BlueToolFixup.kext`，在其他低版本仍然采用 `IntelBluetoothInjector.kext`

## FAQ

### Instruction 使用说明

- 建议所有机友更新 `BIOS` 和 `ME` 固件，链接: <https://pan.baidu.com/s/1-YOJ03WB5NMPURKfWB6RDA> 提取码: 4dga
  - 更新完后记得检查 `BIOS` 中的 `安全启动` 是否已经关闭
  - 请通过 `BIOS` 直接引导 `Windows` 运行更新程序（ `OpenCore` 会注入苹果 `SMBIOS` 信息导致更新程序无法通过验证）
- **由于 ACPI 部分文件变动较大，请先删除原来的 EFI 文件再拷贝新的 EFI 进去，避免出现异常错误导致无法开机**
- **请不要随意更改 `SMBIOS`，因为这会影响到机型的识别，核显的驱动以及 USB 的定制，除非你自己会修改这些东西**
- 关于 `CFG Lock`，可采用 `ru.efi` 对 `BIOS` 进行修改解锁，根据我的测试，本机器的地址码为 `0x3C`，所在属性分支为 `CpuSetup`，如有需求请务必仔细查阅操作说明后再行操作，否则可能出现电脑变砖的严重后果

### OpenCore 多系统引导说明

通过 ACPI Hotpatch，OpenCore 现在已经可以成功引导其它系统了，注意事项如下：

- 需要自己手动根据电脑的 `硬件UUID` 修改配置文件中的 `SmUUID`，否则很可能影响你的 `Windows` 系统以及电脑上众多软件的激活情况
- 通过 `OpenCore` 引导的 `Windows` 系统将被识别为你 `SMBIOS` 所选的苹果机型，若造成系统运行不稳定的情况请自行调试
- 如果需要双系统无缝切换，建议在 BIOS 中将 `OpenCore` 设置为第一引导

### 硬件相关

- 若进入 macOS 前插着耳机，进入系统后耳机音量会出现偏小的情况，重新插拔耳机，音量恢复正常
- 热启动 macOS 很容易出现一些硬件问题，通常是由声卡引起（经核查是由 Windows 下的 B&O 音效驱动引起的），建议是尽量不要使用热启动进入 macOS 系统
- 独显 GTX1050/1050Ti/RX460 和 HDMI 问题
  - 由于英伟达独显的 `VBIOS` 集成到了主板 `BIOS` 中，无法直接读取，因此无法驱动独显，目前未能找到解决方案（HDMI接口挂在独显上，所以也不能外接显示器）
  - 使用 RX460 独显的型号据说是免驱的，主要在同模具的 `Pavilion` 型号上
- 读卡器
  - EFI 不带驱动，需要使用请自行寻找驱动（Sinetek-rtsx.kext）

### 系统相关

- Windows 和 macOS 时间不同步
  - Windows 中打开命令行，输入 Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
  - 重启到 macOS，联网同步时间，再次进入 Windows 即可正常显示时间
- 如果你使用的是 SATA SSD，请打开终端输入 `sudo trimforce enable` 手动开启 `TRIM`
- 如果你想使用 `iMessage` 和 `FaceTime`，需要修改 `序列号` 和 `MLB`，请自行查找相关教程

## 如果你有发现任何新问题，请提交issue，我将跟进修复

- 感谢 `acidanthera` 的团队为黑苹果研究做出的贡献
- 感谢 黑果小兵、宪武、Xjn 等提供的教程和技术支持
- 感谢 开发者 `Goshin` 协助修复 `Synaptics PS2` 触摸板问题
- 感谢 开发者 `zxystd` 在 `Intel` 网卡驱动开发上做出的努力
