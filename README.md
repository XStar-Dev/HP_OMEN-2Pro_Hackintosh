# Hackintosh EFI for HP OMEN 2Pro (15-ax200 Laptop Series)

## Configuration 配置信息

> 本人机型：OMEN by HP Laptop 15-ax225TX

| 硬件 | 型号 | 备注 |
| ----- | ------ | ------ |
| 主板 | HP 8259 ( BIOS Version F.53 ) | 2020.1 更新 |
| CPU | i5 7300HQ 2.5GHz 4C4T  | 800-3500MHz |
| 内存 | 8Gx2 DDR4 2400MHz | 额外加了一条 8G |
| SSD | 西数黑盘 NVMe 二代 SN700 250G | NVMe SSD 默认开启 TRIM |
| HDD | HGST 1T 7200r | \ |
| 显卡 | Intel HD Graphics 630 + NVIDIA GeForce GTX 1050Ti | 通过 SSDT 关闭独显 |
| 声卡 | Realtek ALC295  | A171 - CM238 HD Audio |
| 网卡 | Realtek RTL8111H + BCM94352z  | 暂时换下原装的 7265AC |

## 关于 CLOVER

- `acidanthera` 的团队宣布他们后续版本的 `Lilu`、`VirtualSMC` 等内核扩展程序将不再在 `CLOVER` 上进行测试，无法保证 `CLOVER` 上驱动的兼容性
- `CLOVER` 的 EFI 文件现在已经打包添加到本仓库的 `Release` 专栏（不定期更新，除非有重要修复和调整，针对国内网络，附有网盘链接），同时从仓库代码中移除相关文件

## Recent Updates 最近更新

- 合并 ACPI SSDT，简化代码，已彻底修复电池状态刷新异常问题，并使用 `ACPIBatteryManager` 来管理电池（更稳定）
  - 重要提示：更新本次 EFI 后请务必断电重置一次电池，并先进入 `Windows` 再进入 `macOS`，之后就不需要了，基本正常使用
  - 原理解释：惠普用于机械硬盘保护的加速度传感器 `ACEL` 无法在 `macOS` 下驱动，但是它仍能向 `EC` 中频繁读写数据，影响了电池状态同步，禁止 `ACEL` 设备后电量状态改变正常
- 添加 `CPUFriend` 将闲时频率从 1.2GHz 降低到 0.8GHz，并更改 `EPP` 为节能模式（0x80）
- 更新 `VoodooInput` 和 `VoodooPS2`，已修复偶发性抽风问题，基本完美使用了（受硬件限制，4指手势依然不支持）
- 添加 10.15 可用的 `VoodooHDA` 作为声卡驱动替代方案（`AppleALC` 定制存在问题，当使用电池睡眠唤醒时，会造成 CPU 满载导致耗电增加，甚至造成卡顿）

## Progress 进展

- 引导方式
  - `CLOVER` 已支持，具体版本号见 `Release`
  - `OpenCore` 已支持，运行版本 `0.5.6`
- 系统支持
  - 理论支持 macOS High Sierra 10.13.6（17G65）- Catalina 10.15.4（19E266）之间的全部版本
  - 建议安装 macOS Mojave 10.14.6 或更高版本
- 电源管理
  - CPU变频：已启用 X86 原生电源管理，将闲时频率从 1.2GHz 降低到 0.8GHz，并更改 `EPP` 为节能模式（0x80）
  - 睡眠和唤醒：支持合盖睡眠和唤醒，支持 USB 设备唤醒（当使用电池进行睡眠时，所有外置设备将自动断电无法进行唤醒），`CLOVER` 下可能出现 `RTC` 唤醒（如果开启电能小憩）
  - 电池显示：电量百分比可显示，充放电正常，开启 X86 原生电源管理后状态栏百分比多于实际值，目前尚不明确具体原因，因此从100%满电开始放电时电量变化将会比较缓慢（即便实际值已经是95%状态栏仍然显示为100%）
- 核显 HD630（0x591B8086）
  - 显示正常，支持完整的 `H.264` 和 `HEVC` 解码
  - 支持亮度调节和保存
  - 支持 `FN` 快捷键调节亮度
  - 如需使用 `HiDPI` 请参考网上的教程，使用脚本开启容易出现花屏问题
- 硬盘
  - 添加 `Intel 10 Series AHCI` 识别
  - 添加 `NVMeFix.kext`，开启 NVMe 固态硬盘的高级电源管理（`APST`），能够较为显著地改善 `macOS` 下 `NVMe` 固态硬盘温度过高的问题，我本人测试平均降低约10°左右
- USB
  - 已经正确定制所有端口，USB3.0正常，端口显示最大速率 5 Gbps（如果有时候插上不显示为 5 Gbps，属正常现象，因为 `Windows` 下同样会出现这种情况，是接口接触问题）
  - 内建设备：摄像头（HS04）可用，蓝牙（HS07）
- 网卡
  - 有线网卡：Realtek RTL8111 千兆网卡，正常工作，10.15版本下由于驱动自身原因可能出现偶发性的断网后立马重连的情况
  - 无线网卡：Intel Wireless 7265AC
    - WiFi：目前有众多开发者加入到开发的行列中，相关测试正在进行中，但仍然无法正常使用（如果不想换网卡可用 USB WiFi 替代实现无线上网）
    - 蓝牙：目前已经有大神完成了蓝牙固件上传驱动的开发，本仓库已内置最新编译版，注意存在一个问题：从电脑端发送文件时请从状态栏选择设备发送，若从设备面板发送会出错，原因未知
    - 相关驱动开发项目地址：WiFi [itlwm](https://github.com/zxystd/itlwm)（项目众多，这是其中之一），蓝牙 [IntelBluetoothFirmware](https://github.com/zxystd/IntelBluetoothFirmware)
- 声卡
  - `AppleALC` 基于黑果小兵 `ALC295` 的 `layout-id=13` 修改，已重新定制相关的布局文件，修复热启动时 CPU 占用过高问题
    - 外放和耳机自动切换，麦克风需要手动切换（个人认为麦克风没必要自动切换）
    - 使用电池睡眠唤醒时，会导致 `kernel_task` 进程 CPU 占用异常，造成耗电高且可能卡顿
  - 添加 `VoodooHDA` 替代方案，可正常使用，使用电池时，睡眠唤醒后不会造成 CPU 占用过高问题（`VoodooHDA` 外放声音较小，需要调整参数，具体请自行查找资料）
  - 当前 `OpenCore` 默认使用 `VoodooHDA`，音频输出（外放或者耳机）需手动切换，请注意
  - 热启动外放无声问题是由于 Windows 的声卡驱动造成的，并不是什么特殊情况，很多机器都有，似乎热启动之前拔掉耳机就行
- 触摸板
  - 使用支持 `MT2` 引擎的 `VoodooPS2` 驱动，可使用原生手势（单指轻点、双指滑动、三指滑动调度中心和 App Expose）
  - 四指张合手势受硬件限制暂时无法使用，但是已经在进一步的开发测试中了，由于 `Synaptics PS2` 触摸板自身问题导致手指信息报告异常，需要等大佬解决。

## Instruction 使用说明

- 建议所有机友更新 `BIOS` 和 `ME` 固件，链接: <https://pan.baidu.com/s/1-YOJ03WB5NMPURKfWB6RDA> 提取码: 4dga
  - 请通过 `BIOS` 直接引导 `Windows` 运行更新程序（`OC` 会注入苹果 `SMBIOS` 信息导致更新程序无法通过验证）
  - 更新完后记得检查 `BIOS` 中的 `安全启动` 是否已经关闭
- **由于 ACPI 部分文件变动较大，请先删除原来的 EFI 文件再拷贝新的 EFI 进去，避免出现异常错误导致无法开机**
- **请不要随意更改 `SMBIOS`，因为这会影响到机型的识别，核显的驱动以及 USB 的定制，除非你自己会修改这些东西**
- 通过 ACPI Hotpatch，OpenCore 现在已经可以成功引导 `Windows` 了，注意事项如下：
  - 需要自己手动根据电脑的 `硬件UUID` 修改配置文件中的 `SmUUID`，否则很可能影响你的 `Windows` 系统以及电脑上众多软件的激活情况
  - 通过 `OpenCore` 引导的 `Windows` 系统将被识别为你 `SMBIOS` 所选的苹果机型，若造成系统运行不稳定的情况请自行调试
  - 如果需要双系统无缝切换，建议将 `OpenCore` 设置为第一引导

## FAQ

- 独显 GTX1050/1050Ti/RX460 和 HDMI 问题
  - 由于独显的 `VBIOS` 集成到了主板 `BIOS` 中，无法直接读取，因此无法驱动独显，目前未能找到解决方案（HDMI接口挂在独显上，所以也不能外接显示器）
- 无线网卡 替代方案说明
  - DW1560（BCM94352Z）测试通过，基本没有问题，但是目前价格虚高，谨慎购买
  - DW1820A（BCM94350ZAE）热启动正常工作，冷启动硬件由于不明原因造成机器卡死（猜测是无线部分初始化失败导致：当引导参数添加 `brcmfx-driver=0` 即禁用博通网卡驱动加载时，冷启动可正常进入系统，蓝牙设备也能正常工作，取消引导参数再度重启恢复驱动加载，不再卡死，可正常使用）
  - 白果拆机卡（BCM94360cs2）完全免驱，但是我们的机子空间受限，如果你动手能力强可以选择飞线，没有这个技术就算了，操作不当的话，可能会损坏其它硬件
  - `BrcmPatchRAM3.kext` 仅适用于 `macOS Catalina`，如果使用低版本系统出现问题，请更换合适的驱动，并记得更改 `config.plist` 中的 `Kernel` - `Add` 列表的对应项
- 读卡器
  - EFI 不带驱动，需要使用请自行寻找驱动（Sinetek-rtsx.kext）
- Windows 和 macOS 时间不同步
  - Windows 中打开命令行，输入 Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
  - 重启到 macOS，联网同步时间，再次进入 Windows 即可正常显示时间
- 使用 SATA SSD 时，请打开终端输入 `sudo trimforce enable` 手动开启 `TRIM`
- `macOS` 下的 `iMessage` 和 `FaceTime`，通常需要修改 `序列号` 和 `MLB`，请自行查找相关教程

## 如果你有发现任何新问题，请提交issue，我将跟进修复

- 感谢 `acidanthera` 的团队为黑苹果研究做出的贡献
- 感谢 黑果小兵、Xjn、宪武等提供的教程和技术支持
- 感谢 开发者 `zxystd` 在 `Intel` 网卡驱动开发上做出的努力
