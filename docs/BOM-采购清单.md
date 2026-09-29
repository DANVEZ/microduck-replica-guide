---
title: Microduck 机器鸭 · 复刻 BOM 采购清单
date: 2026-09-29
tags: [机器鸭, Microduck, DIY, BOM, 3D打印, 机器人]
source: fanhao375/microduck-replica + microduck-replica-cad（第三方复刻仓）
---

> ## ⚠️ 请先读：[`DISCLAIMER.md`](../DISCLAIMER.md)
>
> **本仓库是纯网络资料整理，没有任何人按照这些内容实际制造或测试过一台机器鸭。**
> 所有参数、价格、步骤均转述自第三方公开来源，**未经实物验证**。
> 数据截至 **2026-09-29**，开源生态迭代很快，**请以各上游仓库最新状态为准**。
> 把本文当**地图**用，别当**说明书**用。

---

# Microduck 机器鸭 · 复刻 BOM 采购清单

> 整理日期 **2026-09-29**。数据来自第三方复刻仓库（fanhao375 系列）+ 官方开源工程交叉核对。
> ⚠️ **官方从未公布过 BOM**，本清单是从官方 MJCF 仿真模型、STL、Rust 源码、KiCad 工程反推的。
> 价格随地区/时间波动大，**只作量级参考**，下单前自己核价。

---

## 0 · 先做这个决定：两条路线，二选一

**这一步没定，后面所有采购都是错的。两款舵机的 3D 打印配合件不能混用。**

| | **路线 A · 原版 XL330** | **路线 B · 国产飞特 HD-1910** |
|---|---|---|
| 舵机 | Dynamixel **XL330-M288-T** ×15<br>⚠️ 子型号是社区推断，官方源码只写 `xl330` | 飞特 **HD-1910-C001** ×15 |
| 打印件 | CAD 仓库 **v1.1 XL330 版** | CAD 仓库 **v2.1 飞特版**（60 个 SW + 10 STEP，`-FT` 后缀） |
| **舵机单价** | ¥95–299（国内）<br>ROBOTIS 国际站 $23.90 / 美国站 $27.49 | ✅ **¥123/颗**（2026-09 预售）<br>海外零售 $35 |
| **舵机总价（15颗）** | **¥3,600 – 4,500**（国内 ¥299×15）<br>约 $359 – $412 / €603 – 629 | **≈ ¥1,845** —— **约为 XL330 的 40%** |
| 力矩（堵转 @6V） | 6.1 kg·cm（0.60 N·m） | **12 kg·cm（1.18 N·m）—— 约 2 倍** |
| 重量 / 尺寸 | 18 g / 20×34×26 mm | 21±2 g / 20×34×23 mm |
| 减速比 | 288.4 : 1 | 320 : 1 |
| 工作电压 | 3.7–6.0 V（推荐 5 V）<br>⚠️ 官方**超压**跑 6.6–8.2 V | ✅ **4–8.4 V**，2S 直供正好在范围内 |
| 编码器 | 12-bit **绝对式**（AS5601） | 12-bit **磁编码** |
| 电机 / 齿轮 | 有芯 / 工程塑料 | **空心杯 / 金属齿** |
| 空载转速 @6V | 123 rpm | 92 rpm |
| 堵转电流 @6V | 1.74 A | 1.6 A（**15 路峰值预算按 ~10 A 算**） |
| **针序** | `1=GND / 2=Vcc / 3=DATA` | ⚠️ **完全相反**：`1=Signal / 2=Vcc / 3=GND` |
| 官方 9 个 ONNX 策略 | ✅ **直接跑，不用重训** | ❌ **必须重训**（力矩曲线/质量/减速比都不同） |
| 软件改动 | 无，跑官方 `microduck` 主仓即可 | ⚠️ **见下方协议争议**<br>① fanhao375 已写 `feetech` 分支（`bus_feetech.rs` 744 行，**尚未上板**）<br>② 另一说 HD-1910 可通过 XL330 协议/寄存器驱动、robotd 无需改动 —— **两个来源冲突，采购前先实测** |
| 舵机位置计数方向 | 基准 | ⚠️ **15 颗全反**，方向表全取 −1 |
| 实物验证 | — | ✅ 2026-09-13 已整机装出，15 颗全在位 |

### 怎么选

- **想要"开机就能走"** → 路线 A。舵机贵，但官方策略拿来即用，机械件不用重做，风险最低。
- **想省钱 / 想调力矩 / 能接受重训** → 路线 B。省下的舵机钱很大，代价是要有 CUDA 显卡（或 HF jobs）重训策略，且飞特适配仍在"未上板"状态。
- **两条路线共同的坑**：`imu_to_dxl` 板全网无公开工程，**必须自己画**（见 BOM 第三节）。

---

## 1 · 3D 打印件（35 项）

> 件号与螺丝对应见 `../hardware/装配BOM-机械行者Robo-2026-09-16-带采购链接.xlsx`，
> 或 CAD 仓库的 [装配 BOM](https://github.com/fanhao375/microduck-replica-cad#装配-bom)。
> 打印工程：`../hardware/microduck-飞特版-BambuStudio.3mf`（5 盘 / 52 对象，2026-09-15 版）。

| # | 零件 | 文件名 | 材料 | 数量 | 备注 |
|---|---|---|---|---|---|
| 1 | 头部上壳 | `top_head_shell` | PLA | 1 | |
| 2 | 头部下壳 | `bottom_head_shell` | PLA | 1 | |
| 3 | 面部零件 | `face_part` | PLA | 1 | ToF 开孔按 L5CX 板型做的 |
| 4 | 下巴 | `jaw` | PLA | 1 | |
| 5 | 软下巴 | `jaw_soft` | **TPU** | 1 | 软性件 |
| 6 | 软嘴顶部 | `soft_mouth_top` | **TPU** | 1 | 软性件 |
| 7 | 眼睛 | `noenoeil` | PLA | 1 | |
| 8 | 颈部 | `neck` | PLA | **2** | 可换铝合金增强 |
| 9 | 颈部俯仰 | `neck_pitch` | PLA | 1 | |
| 10 | 躯干底座 | `trunk_base` | PLA | 1 | 可换铝合金增强 |
| 11 | 左壳 | `left_shell` | PLA | 1 | |
| 12 | 右壳 | `right_shell` | PLA | 1 | |
| 13 | 髋部左 | `hip_l` | PLA | **2** | |
| 14 | 偏航转横滚 | `yaw2roll` | PLA | 1 | |
| 15 | 偏航横滚运动 | `yaw_roll_motion` | PLA | 1 | |
| 16 | 腿部 | `leg` | PLA | **2** | |
| 17 | 左上腿 | `left_upper_leg` | PLA | 1 | **镜像件，别装反** |
| 18 | 右上腿 | `right_upper_leg` | PLA | 1 | **镜像件** |
| 19 | 上腿加固板 | `upper_leg_rigidity_plate` | PLA | **2** | 可换铝合金 |
| 20 | 左脚 | `foot_left` | PLA | 1 | |
| 21 | 右脚 | `foot_right` | PLA | 1 | |
| 22 | 左脚底 | `sole_left` | PLA | 1 | |
| 23 | 右脚底 | `sole_right` | PLA | 1 | |
| 24 | 脚踝左 | `ankle_left` | PLA | 1 | 与 26 二选一 |
| 25 | 脚踝右 | `ankle_right` | PLA | 1 | 与 27 二选一 |
| 26 | 脚踝左 V1（备选） | `ankle_l_v1` | PLA | 1 | 备选版 |
| 27 | 脚踝右 V1（备选） | `ankle_r_v1` | PLA | 1 | 备选版 |
| 28 | 轮辋 | `rim` | PLA | 4 | 🔵 **轮滑变体，走路不用** |
| 29 | 轮胎 | `tire` | **TPU** | 4 | 🔵 轮滑变体；v2.1 已改为 TPU 合体轮胎 |
| 30 | 滚轮叶片 | `roller_blade` | PLA | 2 | 🔵 轮滑变体 |
| 31 | M12 镜头座 | `m12_lens_holder` | PLA | 1 | |
| 32 | 电机支架 | `motor_support` | PLA | 1 | |
| 33 | 电源支架 | `power_support` | PLA | 1 | |
| 34 | 香蕉形 PCB 锁扣 | `banana_pcb_locker` | PLA | 1 | |
| 35 | 轴承滚轮 | `bearing_roll` | PLA | **2** | 是压盖不是轴承，**要打印不用买** |

**走路只要 1–27 + 31–35。** 28/29/30 是做轮滑功能才打。

**下载**：
- 飞特版 v2.1：`SolidWorks-FT2026-09-25.zip`（343 MB） 或只要改动件 `STEP-changed-parts-FT-2026-09-25.zip`（10 MB，不用 SolidWorks 也能开）
- XL330 版 v1.1：`SolidWorks-XL330-v1.1.zip`（333 MB）
- 压缩包都在 [CAD 仓库 Releases 页](https://github.com/fanhao375/microduck-replica-cad/releases)，**不在文件列表里**

---

## 2 · 外购件 · 机械与标准件

| # | 件 | 规格 | 数量 | 参考价 | 链接 / 渠道 |
|---|---|---|---|---|---|
| 36 | 轴承 | **Ø10×15×3** | **3** | ¥5/个 | [淘宝 963037239628](https://item.taobao.com/item.htm?id=963037239628&skuId=6069280211062)（选 10×15×3 SKU） |
| 37 | 轴承 | **Ø16×22×4** | **11** | ¥2.2–5 | [天猫 978199812185](https://detail.tmall.com/item.htm?id=978199812185&skuId=6245616343131) |
| 38 | 轴承（轮滑用） | Ø6×12×3 | 2 | — | 同 36 链接换 SKU；🔵 走路不用买 |
| 39 | 螺丝套装 | M2×5 / M2×6 / M2.5×6 | 见下 | ¥27 | [天猫 637524754721](https://detail.tmall.com/item.htm?id=637524754721) 按规格选 SKU |
| 40 | 热熔螺母 | **M2**（+M3 混装） | 400 pcs/盒 | ¥22 | [淘宝 1000673642588](https://item.taobao.com/item.htm?id=1000673642588) |
| 41 | 热熔螺母压头 | M2–M8，适用 936/T12/T65 | 1 套 | ¥13 | [淘宝 902798112263](https://item.taobao.com/item?id=902798112263) ⚠️ **别忘买，普通烙铁头压不正** |
| 42 | 螺纹胶 | 乐泰 **243** 中强度 | 1 | ¥19 | [天猫 653848839737](https://detail.tmall.com/item.htm?id=653848839737) ⚠️ **低于 ¥15 的当心假货** |

### 螺丝用量（按孔位加权共 237 个孔）

| 规格 | 建议买 | 用途 |
|---|---|---|
| M2×4 内六角圆柱头 | 60 | 薄壁位 |
| **M2×6 内六角圆柱头** | **80（主力）** | |
| M2×8 内六角圆柱头 | 40 | 孔深 3–5 mm |
| M2×12 内六角圆柱头 | 15 | 少量深孔 |
| M2 螺母 | 50 | 无攻丝处 |
| **M2 热熔螺母** | **60** | 打印件推荐，比直接攻丝牢得多 |
| M2.5×6 | 20 | 少量 Ø2.7 孔位 |

> 💡 也可以直接买 **600 pcs M2/M2.5/M3 组合套装**（[淘宝 842110292995](https://item.taobao.com/item.htm?id=842110292995)，¥26.8 包邮送扳手）。
> ⚠️ 很多「M2–M8 大全」标题带 M2 实际最小只到 M3，**下单前看清 SKU**。本项目 M2 是绝对主力，M3 基本用不上。

---

## 3 · 外购件 · 电子件（核心）

| # | 件 | 型号 / 规格 | 数量 | 参考价 | 备注 |
|---|---|---|---|---|---|
| 43 | **主控** | **Radxa Zero 3W · 2G/16G eMMC** | 1 | **¥237** | ⚠️ 见下方渠道警告 |
| 44 | **舵机** | Dynamixel XL330-M288-T **或** 飞特 HD-1910-C001 | **15** | 见路线表 | 13 单盘 + 2 双盘 |
| 45 | 舵机线缆 | 飞特 2.0 mm / Dynamixel 2.5 mm | 17 | ¥1.8–2.5 | 舵机自带一根，调试和总线分支要另买 |
| 46 | 电池 | **索尼 NP-F550**，2S 7.4 V | 1 | ¥52.8 | ⚠️ **不是 F970！**见下 |
| 47 | 电池座 | NP-F 电池仓 + 取电扣板 | 1 | ¥19–37 | [天猫 658975825526](https://detail.tmall.com/item.htm?id=658975825526) + [天猫 926961598924](https://detail.tmall.com/item.htm?id=926961598924) |
| 48 | 摄像头 | **IMX219**（树莓派 Camera v2） | 1 | ¥32.8 | [天猫 775872575316](https://detail.tmall.com/item.htm?id=775872575316) |
| 49 | 摄像头排线 | **15P 1.0 → 22P 0.5 同面触点**，**4–15 cm** | 1 | ¥1.88–2.5 | [淘宝 654755002992](https://item.taobao.com/item.htm?id=654755002992)（4 cm 短版）⚠️ **别买 30 cm** |
| 50 | **ToF 测距** | **VL53L5CX**（VL53L5X V2 模块，15.8×10 mm） | 1 | ¥44.5 | ⚠️ **只有 L5CX / L8CX 固件认**，见下 |
| 51 | 扬声器 | 5 W 小喇叭 | 1 | ¥5 | 只在打 HAT / 走 USB 声卡时需要 |
| 52 | 半双工转接板 | 串口总线舵机驱动板（ST/SC 系列） | 1 | ¥22 | [淘宝 1005305041207](https://item.taobao.com/item.htm?id=1005305041207) ✅ **不打 HAT 时的必需件** |
| 53 | 降压模块 UBEC | 5V/3A，**输入下限 ≤6 V**，同步降压 | 1 | ¥10–15 | ✅ **不打 HAT 时给主控供电** |
| 54 | 高耐久 microSD | SanDisk High Endurance / 海康 PLUS 64G | **2** | ¥90×2 | ⚠️ **不是** Ultra/EVO 那种普通卡；买两张，一张备份 |

### ⚠️ 主控渠道警告

官方分销商 ALLNET China 实价：**1G/无eMMC $18 / 1G+8G $22 / 2G+16G $32.90（≈¥237）/ 4G+32G $50**。
国内电商**全线溢价 3–6 倍**（findboard 8G+64G 套餐 ¥1469、裸板 ¥1149；鹭控 ¥690）。
→ 走 [ALLNET China](https://shop.allnetchina.cn/products/copy-of-radxa-zero-3w)、速卖通或瑞莎官方渠道。
→ 官方公布配置是 **1G/32G**，但**复刻建议 2G/16G**（1G 要和 zram 日志设备分内存，跑起来偏紧）。
→ ⚠️ **2026-09 断货严重**，1G 版官方 $18 被炒到 ¥700。**这块板是复刻进度的最大瓶颈。**

### ⚠️ 电池型号勘误：是 NP-F550，不是 F970

上游网格文件名叫 `np_f970`，**但这是误导**：网格实测包围盒 **70.8 × 38.6 × 20.6 mm**，正是 **NP-F550/F570** 尺寸。
真 NP-F970 厚约 **60 mm**、重约 300 g —— **买错装不进去**，而且 300 g 会吃掉整机 800 g 预算的三分之一以上。
源码全库检索只有 NP-F550，F970 零命中。

### ⚠️ ToF 选型：先看芯片，再看尺寸

| 型号 | 板子尺寸 | 固件认不认 | 结论 |
|---|---|---|---|
| **VL53L5CX**（VL53L5X V2） | **15.8 × 10 mm**，孔距 **20 mm** | ✅ `0x02` | ✅ **首选**：固件认、板最小、还最便宜 |
| VL53L8CX（国内常见板型） | 33.4 × 10.2 mm，孔距 28 mm | ✅ `0x0C` | ⚠️ 芯片没问题，**但这块板 脸壳装不下** |
| VL53L7CX | 26 × 11.5 mm | ❌ 直接报错退出 | ❌ **别买**，L7 是 90° 宽视场的**另一颗芯片** |

**飞机不要买成 VL53L0X** —— 那是**单点**测距，固件不认；搜「VL53L5CX」出来大多是 L0X。

---

## 4 · 两块电路板

### 板 1：RPI Robot HAT —— 官方开源，下载即打样

| 项 | 值 |
|---|---|
| 来源 | [`pollen-robotics/elec_RPI_Robot_HAT`](https://github.com/pollen-robotics/elec_RPI_Robot_HAT)（Apache-2.0，`production/` 含 Gerber+BOM+坐标） |
| 层数 / 板厚 | **4 层** / **1.0 mm** |
| 尺寸 | **65.0 × 30.9 mm**，圆角 R3.5 |
| BOM | 47 行 / 123 颗，其中 5 行 9 颗 DNP 不贴 → 实际贴装 **113 个位置** |
| 手焊可行性 | ❌ 不可能（含 VQFN-32 codec + LGA-16 IMU），**必须选 SMT 服务** |
| 成本 | 几百元起，有起订量 |
| ⚠️ | 打开 KiCad 工程需先装 [`lib_KiCAD`](https://github.com/pollen-robotics/lib_KiCAD)；**只打样不需要** |

**功能四块**：① 配电（`+BATT` 直供舵机 + AP63205 出 5V 给主控）② 舵机半双工 TTL 总线 ③ 音频（codec + 功放 + MIC）④ 传感器扩展（Stemma/Qwiic 接 ToF）。
⚠️ **这块板没有充电电路也没有 USB-C 输入**，电池要用外部充电器充。**Pollen 不单卖。**

### 板 2：`imu_to_dxl` —— 全网无公开工程，**必须自己画**

把 IMU 伪装成 Dynamixel 从机（**ID 200**）挂在舵机总线上，让主机一个 tick 只发**一次 `sync_read`** 就同时拿回 IMU 姿态 + 15 个舵机位置，**时间戳天然对齐**。

**参考 BOM**（第三方设计，非官方）：

| 位号 | 器件 | 立创编号 |
|---|---|---|
| U1 | STM32G031F8P6（TSSOP-20） | `C529334` |
| U2 | LSM6DSV16XTR（六轴 IMU + SFLP 硬件融合） | `C5267406` |
| U3 | SN74LVC2G241DCUR（三态缓冲，单线半双工） | `C10430` |
| U4 | HT7533-1（3.3V LDO，**耐压 30 V**） | `C14289` |
| J1/J2 | B3B-EH-A(LF)(SN) Dynamixel 3P 2.5mm | `C160259` |
| J4/J5 | B3B-PH-K-S(LF)(SN) 飞特 3P 2.0mm ⚠️料号待定 | — |
| J3 | PZ254V-11-06P（SWD + 串口 printf 合一） | `C492405` |
| C1–C4,C8,C9 | 100 nF 0603 | `C14663` |
| C5 | 4.7 µF/16V 0603 X5R | `C19666` |
| **C6** | **10 µF/25V 0805 X5R** ⚠️ **必须 ≥25 V** | `C15850` |
| C7 | 10 µF 0603 10V | `C19702` |
| R1/R2/R4/R5/R7 | 10 kΩ 0603 | `C25804` |
| R6 | 150 Ω 0603 | `C22808` |
| D2 | BZT52C5V1-7-F **SOD-123** 5.1V 稳压管 | `C151588` |
| D1 | TVS **SMF12A** SOD-123FL | `C2943870` |
| F1 | PPTC **MF-NSMF020X-2** 1206 | `C210358` |
| X1 | **F322516MUBCE2O** 16 MHz **有源晶振** | `C5917307` |
| R3 | 33 Ω 0603 | `C23140` |

**规格**：2 层板 · 45 × 22 mm（宽松）· 两个 M2 安装孔间距 **34 mm** · 打样约 ¥25–40（不含贴片）。

**三条硬约束**：
1. **LDO 耐压**：总线满电 8.4 V，常见型号全部不够（AP2112K/ME6211 只有 6.0 V）→ 必须 HT7533-1 或同级
2. **C6 输入电容 ≥25 V**：10 V 料挂在 8.4 V 上余量只剩 1.2×，且 X5R 有直流偏压衰减
3. **⚠️ 换晶振只认「有源晶振/OSC」标签**：3225 四脚的无源晶体 2/4 脚是同一片金属盖，贴上去**一上电就是 3.3 V 对地短路**（`C13738` 最容易顺手点错）

---

## 5 · 线材、工具与耗材

### 线材与配件（不打 HAT 时）

| 件 | 规格 | 约价 | 买之前看 |
|---|---|---|---|
| 2×20 排针 | 2.54 mm 立式，**买彩色的好数脚** | ¥1 | Radxa 2G 无 eMMC 版**没焊排针** |
| 硅胶线 | **18–20 AWG** 红黑各 1 m | ¥10 | ⚠️ **舵机主干别用杜邦线**，15 颗峰值好几安培 |
| 保险丝座 + 管 | 5×20 座带线 + **5 A 快熔** | ¥5 | 锂电池短路会喷火，必须串 |
| 电源开关 | 船型开关 KCD1，额定 ≥6 A | ¥3 | |
| PH2.0 3P 端子线 | 2.0 mm 3 针单头带线 | ¥5 | 配飞特舵机线用 |
| 杜邦线 | 母对母 20 cm，一排 40 根 | ¥5 | 只用于信号，不用于电源主干 |
| 电解电容 | **470 µF / 16 V** 105℃ | ¥1 | 并在降压模块**输入端**，缓冲舵机启动压降 |
| 读卡器 | USB 3.0 TF | ¥15 | |
| *(可选)* USB 声卡 | **免驱**、带麦克风口 | ¥15 | 替代 HAT 音频 |
| *(可选)* 小喇叭 | 4 Ω 3 W，或直接买带功放的 USB 小音箱 | ¥5 | |

### 工具（一次性投入）

| 工具 | 用途 | 必要性 |
|---|---|---|
| 内六角螺丝刀组 | M2/M2.5 | ✅ 必需 |
| 镊子 | 装小件 | ✅ 必需 |
| 电烙铁 + **热熔螺母压头** | 压热熔螺母 | ✅ 必需 |
| **FE-URT-2 调试板**（[官方店 ¥45](https://item.taobao.com/item.htm?id=603181554943)） | 配舵机 ID / 台架调试 | ✅ **必需**，舵机到货第一件事就是实测总线时序 |
| 万用表 | 量电压、查线序、验极性 | ✅ **必需** |
| 3D 打印机（Bambu P2S 等）或代打服务 | 打结构件 | ✅ 必需 |
| 螺丝刀（十字）、卡尺 | 装配 / 验孔径 | 建议 |
| SWD 调试器（ST-Link / CMSIS-DAP） | 烧 `imu_to_dxl` 固件 | 自绘板时需要 |

### 打印耗材

| 材料 | 用量 | 用在哪 |
|---|---|---|
| **PLA / PETG** | 约 **300–500 g** | 绝大部分结构件 |
| **TPU** | 少量 | `jaw_soft` 软下巴、`soft_mouth_top` 软嘴顶部、轮胎 |

---

## 6 · 成本速览

| 类别 | 数量 | 量级 |
|---|---|---|
| **舵机** | 15 | 路线 A：**¥3,600–4,500** ／ 路线 B：**≈¥1,845** ← 绝对大头 |
| 主控与传感器 | 4 类 | 约 ¥580–860 |
| 电池与供电 | 2 件 | 约 ¥220–370 |
| 轴承 | 14 | 约 ¥110–280 |
| 紧固件 | 约 325 件 | 约 ¥110–180 |
| PCB 打样（2 块，含 SMT） | 2 | 约 ¥430–1,100 |
| 打印耗材 | — | 约 ¥110–220 |
| 工具（一次性） | — | 约 ¥200–400 |
| **合计（路线 A · XL330）** | | **约 ¥5,400 – 8,000** |
| **合计（路线 B · 飞特）** | | **约 ¥3,600 – 6,000** ＋ 需 GPU 重训策略 |

### ⭐ 实测参照：Fanduck 项目真实账单

社区里唯一公开逐项记账的项目（duck.fandcode.com，原版 XL330 路线，2026-09-16 统计）：

| 项 | 值 |
|---|---|
| **可追溯承诺支出合计** | **¥11,923.60** |
| 是否含打板费 | ✅ 含 IMU 与其他板件打板费用 |
| 进度 | 8 个阶段全部"已完成"，含官方 9 项策略全通过 |
| 作者自评 | "计划破产，经费超预算了。鸭子还没站起来，钱包先躺下了" |

**这比上表的估算高出一大截**，差异主要来自：**打板费（HAT 多层板 + 触点板 + NFC 板 + IMU 板，多次改版重打）** 和 **备件冗余**（XL330 买了 16 颗而非 15 颗、多片 HAT、多张卡）。

> 💡 **规划预算时按 ¥10,000–12,000 准备**，尤其是如果你也要自己打 HAT 板。
> 走「不打 HAT」飞线方案能砍掉很大一块。

> ⚠️ **$399 是矽递规模化量产 + 官方供应链的价格，个人复刻不可能达到。**
> 社区复现群给的心理预期是"上千起步，全部搞完整估计可以上万"。
> **这条路买到的不是性价比，是过程本身。**

---

## 7 · 还没解决的空白（复刻到最后会卡在这）

| # | 问题 | 现状 | 建议 |
|---|---|---|---|
| 1 | **电池取电触点** | CAD 里只有打印件 `power_support`（×2），**没有任何触点 PCB 或簧片模型** | 买现成的 **NP-F 电池转接板/取电扣板** |
| 2 | **整机线束** | 官方没有任何走线图纸；舵机 3P 线长、MIPI 排线长都要实测 | 舵机自带短线，装机时量实际长度 |
| 3 | **`imu_to_dxl` 原理图** | 全网无公开工程，功能已从源码完整还原但**要自己画板** | 按本清单第 4 节参考 BOM 自绘 |
| 4 | **换舵机后的策略** | 官方 9 个 ONNX 只对原本体有效 | 用 `microduck_rl` 重训（需 CUDA GPU） |
| 5 | **非商用限制** | 3D 模型许可 **CC BY-NC-SA 4.0** | 个人做没问题，**销售不在许可范围内** |

---

## 8 · 开源资源索引（一站式）

### 官方（pollen-robotics）

| 仓库 | 许可 | 是什么 |
|---|---|---|
| [**microduck**](https://github.com/pollen-robotics/microduck) | Apache-2.0 | **板载运行时**，Rust 写的 50Hz 控制环 / 更新器 / BLE / WebRTC / ToF。**想读硬件规格也看这里 —— 代码即规格书** |
| [**microduck_rl**](https://github.com/pollen-robotics/microduck_rl) | Apache-2.0 | **强化学习训练环境**。⭐ **47 个 STL 和完整 MJCF 在这里**，是所有几何信息的唯一来源 |
| [**elec_RPI_Robot_HAT**](https://github.com/pollen-robotics/elec_RPI_Robot_HAT) | Apache-2.0 | ⭐ **HAT 板完整开源工程**，`production/` 直接打样。⚠️ 容易被漏掉，不在主仓 |
| [lib_KiCAD](https://github.com/pollen-robotics/lib_KiCAD) | — | 打开 HAT 工程前必须先装 |
| [rustypot](https://github.com/pollen-robotics/rustypot) | — | Dynamixel 通信库（Rust），`xl330.rs` 是完整寄存器表 |
| [microduck-gst-plugins](https://github.com/pollen-robotics/microduck-gst-plugins) | — | aarch64 GStreamer 插件 |

> **很多人以为"Microduck 硬件没开源"—— HAT 板其实是开源的，只是不在主仓。**
> 真正没公开的是：`imu_to_dxl` 板、可编辑机械 CAD、整机 BOM。

### Hugging Face

| 名称 | 说明 |
|---|---|
| [**microduck-simulator**](https://huggingface.co/spaces/pollen-robotics/microduck-simulator) | ⭐ **官方网页模拟器，打开就能玩**，不用装东西。**从这开始** |
| [**microduck-policies**](https://huggingface.co/pollen-robotics/microduck-policies) | ⭐ **官方 9 个 ONNX 策略**，硬件同款可直接用 |
| [mishig/microduck-anatomy](https://huggingface.co/spaces/mishig/microduck-anatomy) | 结构解剖可视化 |
| microduck-ar / microduck-vla-simulator | AR 查看 / VLA 模拟 |

### 复刻项目（本清单的数据来源）

| 仓库 | 是什么 |
|---|---|
| [**fanhao375/microduck-replica**](https://github.com/fanhao375/microduck-replica) | ⭐ **装配分析 + 电控逆向**。BOM、电控/机械采购清单、不打 HAT 方案、踩坑记录、飞特适配（**有实物，最硬核**） |
| [**fanhao375/microduck-replica-cad**](https://github.com/fanhao375/microduck-replica-cad) | ⭐ **可编辑 SolidWorks 图纸 + 21 页装配说明书 + 打印 BOM + BambuStudio 工程**（作者：机械行者Robo） |
| [**SaberOnGo/open-microduck**](https://github.com/SaberOnGo/open-microduck) | ⭐ **中英双语研究文档库**（暂无 PCB 文件）。特点：**证据分级严谨** —— 严格区分「官方产品规格 / 官方源码 / 社区重建 / 未确认」四档，避免把社区推断当官方 BOM。含 `robotd` 硬件协议逐项拆解、选型路径、故障排查 |
| [**duck.fandcode.com**](https://duck.fandcode.com/) | ⭐ **「超能花钱鸭 Fanduck」** —— 唯一公开逐项记账的复刻项目。24 项硬件状态追踪、真实花费 **¥11,923.60**、4 个可下载制作资料包（物料清单/板件制造/工具源码/镜像工作台）。原版 XL330 路线，**官方 9 项策略全部通过** |
| [IronSpiderMan/MicroDuckModels](https://github.com/IronSpiderMan/MicroDuckModels) | 中文，Three.js 网页 3D 预览器 |
| [ScrapMeta/microduck-diy](https://github.com/ScrapMeta/microduck-diy) | 中文，「一个月手搓挑战」记录 |
| [fengj4780-sudo/MICDUCK_FTHD1901_REBUILD](https://github.com/fengj4780-sudo/MICDUCK_FTHD1901_REBUILD) | **飞特版结构改进 + 可选 CNC 加强件**（5 件含运费约 ¥45） |
| [Rhoban/microban](https://github.com/Rhoban/microban) | ⭐ **不是 Microduck，但用同一块 HAT**。有完整公开 BOM 和装配指南（约 $567），采购时值得对照 |
| [joeynyc/awesome-microduck](https://github.com/joeynyc/awesome-microduck) | 英文 awesome 列表 |
| [rokbenko/quackd](https://github.com/rokbenko/quackd) | 接大模型，用自然语言指挥 |
| [jonathanhawkins/microduck-lab](https://github.com/jonathanhawkins/microduck-lab) | **在普通 Mac 上训 RL 策略，不需要 CUDA** |

---

## 8.5 · ✅ 已核实结案：HD-1910 必须 fork 运行时

### 结论

**HD-1910 用的是飞特自有 SCS/STS 协议，与 Dynamixel Protocol 2.0 结构上不兼容。走路线 B 就必须用 `feetech` 分支，网上"robotd 不用改一行代码"的说法是错的。**

### 决定性证据

来源：飞特官方文档 `doc.feetech.cn`，副本已归档到 `飞特官方文档（doc.feetech.cn）`
— 《SCS 通信协议》v1.0（2026-06-09）
— 《磁编码 STS 内存表手册》v1.1（2026-08-27）
— 官方 SDK `FTServo_Linux/src/SMS_STS.h`（2025-09-27）
— `rustypot` `sts3215.rs`

| 事项 | XL330（Protocol 2.0） | 飞特 STS/SMS（HD-1910） |
|---|---|---|
| **包格式** | `FF FF FD 00` + ID + LEN + INST + `…CRC16` | `FF FF` + ID + LEN + INST + `…CHK`<br>**CHK = `~(ID+LEN+INST+参数)`** |
| **字头长度** | **4 字节**（`FF FF FD 00`） | **2 字节**（`FF FF`） |
| **校验方式** | **CRC-16** | **逐字节取反和** |
| 波特率寄存器 | 8（`3` = 1 Mbps） | 6（`0` = 1 Mbps，**出厂即 1 Mbps**） |
| 位置寄存器 | 132 读 / 116 写 | **56 读 / 42 写** |
| 速度寄存器 | 128 | 58 |
| 电流 / 电压 / 温度 | 126 / 144 / 146 | 69 / 62 / 63 |
| 扭矩开关 | 64 | 40 |
| 运行模式寄存器 | 11（`3` = 位置） | 33（**出厂 `4` = 纯位置 PD**） |
| 位置环增益 | P84/I82/D80，u16，RAM | **P21/D22/I23，u8，在 EEPROM** |
| EEPROM 写保护 | 无 | **55 lock**（写 1 加锁，写入掉电不保存） |
| **IMU** | 挂总线 **ID 200**，同一次 sync_read | **总线上没有 IMU** |

**字头 4 字节 vs 2 字节、CRC16 vs 取反和 —— 这两条任一都足以判定不兼容，没有任何"协议兼容"的空间。**

### 那个错误说法是怎么来的

内容农场文章（171host 类）把 OpenMicroDuck 文档里的这句话误读了：

> "15 个 Servo 和控制 IMU 共用同一条 **Dynamixel-compatible** Serial Bus"

**这句描述的是官方运行时期望的总线，不是在说 HD-1910 兼容。** 这类文章会用 AI 编造看似精确的技术结论，**只信有实物验证的项目**。

### 走路线 B 的实际工作量（已实测到哪一步）

fork 分支：[`fanhao375/microduck` @ `feetech`](https://github.com/fanhao375/microduck/tree/feetech)（`8db8f00`，2026-09-20 rebase 到上游 0.14.1）

| 已完成 | 状态 |
|---|---|
| `bus_feetech.rs`（新） | ✅ 每 tick 一次 `sync_read` 地址 56 长度 15；目标写 42；`set_gain` 只写 P 且不开锁 |
| `bus_select.rs`（新） | ✅ `AnyBus` 枚举二选一 |
| `robotd-params` | ✅ `[bus] protocol = "dynamixel2" \| "feetech-sts"` |
| 测试 | ✅ 控制库 80 / 参数库 150 / 守护进程 93 **全过**，新增 11 个测试 |
| 交叉编译 | ✅ 11 个程序 aarch64 通过，已用开发密钥签名 |
| **上板** | ❌ **还没上板** |
| **IMU** | ❌ 后端 `imu_ready()` 恒 false → **不启动策略、不做跌倒检测**，只有 `robot init`（2 秒插到站姿）能用 |

### ⚠️ 上板前必须实测的 13 项（作者待查清单，按决定性排序）

1. **寄存器底账** —— FD 编程页导出整颗 0–90 存仓库，逐行对表
2. **运行模式** —— 出厂 `4` 是不是要跑的那个；模式 4 和 0 下 P/D 响应差别
3. **锁着写 P** —— lock=1 时写 21，响应是否立即变、重启是否回原值
4. **0x08 重启** —— 备用舵机上发，1 s 后 ping 通否、ID/EEPROM 保留否
5. 应答级别出厂值 1，确认没被 FD 改过
6. **sync_read 应答顺序** —— 不存在的 ID 排首位时 15 颗的行为
7. 位置寄存器第 15 位在舵机模式下会不会置 1
8. **方向约定** —— 已知 15 颗全反（−1）
9. 字节序：磁编码系列小端，地址 2 `END = 0` 可在线断言
10. 方案 B：BNO085 在 i2c 上 100 Hz 的抖动和丢帧
11. **半双工回显** —— rustypot 收状态包是定长 `read_exact`，不扫包头；若 RX 常通，每笔事务第一包就是自己的指令，**整条总线"不通"，现象像接线错**
12. **台架供电** —— `battery_empty_shutdown` 默认开，总线 ≤ 6.6 V 就坐下关机；HD-1910 有 6 V 档，**台架电源要 ≥ 7 V 或先关这项**
13. **相位 18 出厂值** —— BIT2（速度单位）、BIT3（速度 0 语义）、BIT0/BIT7（方向）

### 上板步骤（板子开机后）

```bash
# 1) 板上信任开发公钥：/etc/robot/trusted_keys/replica.dev.pub
#    /etc/robot/updater.toml 里 allow_dev_keys = true（正式机器是关的，更新不覆盖）
# 2) 先装原版证明部署链路通（此时 robotd.toml 还是 dynamixel2）
robotctl update apply daemon --from <...>
#    有健康检查，起不来自动回滚到 0.13.0
# 3) 改 robotd.toml：
#    port = "/dev/ttyUSB0"          # URT-2 插板子
#    protocol = "feetech-sts"
#    [bus.feetech] directions = [...]   # 15 颗全是 -1
#    重启 robotd，看 robotctl health 的 bus 项
# 4) 鸭子吊起来，再 robotctl robot init
```

---

## 8.6 · 其余待确认项（影响较小）

| # | 争议点 | 说法 A | 说法 B | 怎么验 |
|---|---|---|---|---|
| 2 | **XL330 具体子型号** | 社区普遍写 **M288-T**（依据是减速比 288.4:1） | 官方源码只写 `motor_name="xl330"` **无后缀**；官方 BOM 从未公布子型号 | 不影响采购决策（M288 是唯一合理选择），但**别把它当"官方确认"** |
| 3 | **HD-1910 是否"2.5 倍力矩"** | fanhao375 称"力矩大 2.5 倍" | 飞特规格书：6.1 → 12 kg·cm，**约 2 倍** | 以规格书为准：**约 2 倍** |
| 4 | **正极针序** | fanhao375：`1=Signal / 2=Vcc / 3=GND` | 规格书：同 | 两源一致 ✅，但**接线前万用表点一遍** |
| 5 | **整机重量** | Press Kit：**under 800 g** | 官方商店：**780 g**；社区逆向实测：**737.2 g** | 以 780 g 为准 |

---

## 9 · 本目录文件说明

| 文件 | 内容 |
|---|---|
| `BOM-采购清单.md` | 本文 |
| `SOP-施工流程.md` | **施工 SOP**，从采购到首次走动 |
| `../bom/Microduck-复刻BOM.xlsx` | 可下单的 Excel 版物料表 |
| `../hardware/` | 装配说明书 PDF（21 页）+ 打印 BOM Excel + BambuStudio 3MF + 23 张组件图/爆炸图 |
| `../references/` | 上游 14 份核心中文文档（BOM / 采购清单 / 硬件入门 / 踩坑记录 / 调试记录 / 不打 HAT / 镜像说明 等） |

---

**许可证声明**：3D 模型与图纸来自上游 **CC BY-NC-SA 4.0**（署名 · 非商业性使用 · 相同方式共享）。
官方代码 Apache-2.0。**个人制作没问题，销售不在许可范围内。**
本清单为第三方复刻资料整理，与 Pollen Robotics 无隶属关系。

**数据来源**：`fanhao375/microduck-replica`（BOM.md / 机械采购清单 / 电控采购清单 / 执行器选型 / 紧固件反推 / 生态导航）、`fanhao375/microduck-replica-cad`（README / 装配 BOM / 安装说明书）。整理日 2026-09-29。
