# 数据来源与署名

本仓库是一份**二手资料整理**。全部原始数据来自下列公开来源，版权归各自作者所有。
**请优先阅读原始来源，它们比本仓库更权威、更新更及时。**

---

## 一、核心复刻仓库（⭐ 强烈建议都看）

### fanhao375/microduck-replica

- 地址：https://github.com/fanhao375/microduck-replica
- 作者：**fanhao375**
- 内容：电控逆向、硬件方案逆向、BOM、机械/电控采购清单、紧固件反推、
  「不打 HAT」飞线方案、踩坑记录、调试记录、构建日志、飞特舵机适配方案
- 许可：**双许可** —— `scripts/`、`tools/` 为 **Apache-2.0**；
  `assembly-drawings/`、3D 网格、**全部文档（`*.md`）** 为 **CC BY-NC-SA 4.0**
- 特点：**有实物验证，是社区里最硬核的一份**
- 本仓库中 `references/` 目录下的技术文档即来自此仓库

### fanhao375/microduck-replica-cad

- 地址：https://github.com/fanhao375/microduck-replica-cad
- 作者：**机械行者Robo**
- 内容：可编辑 **SolidWorks 参数模型**、**21 页装配安装说明书**、
  打印 BOM（带采购链接）、**BambuStudio 打印工程**、零件爆炸图
- 许可：**CC BY-NC-SA 4.0**（署名 · 非商业性使用 · 相同方式共享）
- 说明：依据 `pollen-robotics/microduck_rl` 公开发布的 STL 网格重建为可编辑模型。
  **图纸与说明书著作权归「机械行者Robo」**
- 本仓库中 `hardware/` 与 `bom/装配BOM-带采购链接.xlsx` 来自此仓库

### SaberOnGo/open-microduck

- 地址：https://github.com/SaberOnGo/open-microduck
- 作者：**SaberOnGo**
- 内容：中英双语研究文档库（暂无 PCB 文件），含 `robotd` 硬件协议逐项拆解、
  硬件选型路径、故障排查、书籍《让小鸭子迈出第一步》
- 特点：⭐ **证据分级最严谨** —— 严格区分「官方产品规格 / 官方源码 /
  社区重建 / 未确认」四档，避免把社区推断当官方 BOM
- 许可：非商业研究项目，详见其 [来源与许可证](https://github.com/SaberOnGo/open-microduck/blob/main/docs/zh-CN/legal/provenance-and-licenses.md)

### duck.fandcode.com —「超能花钱鸭 Fanduck」

- 地址：https://duck.fandcode.com/
- 作者：**Fandcode**
- 内容：**唯一公开逐项记账**的复刻项目。24 项硬件状态追踪、真实花费明细、
  4 个可下载制作资料包（物料清单 / 板件制造 / 工具源码 / 镜像工作台）
- 实测数据：**可追溯承诺支出合计 ¥11,923.60**（截至 2026-09-16，原版 XL330 路线，
  含全部打板费用），8 个阶段全部完成，官方 9 项策略全部通过
- 本仓库的「真实成本锚点」即来自此项目

---

## 二、官方来源

| 来源 | 地址 | 说明 |
|---|---|---|
| 官方运行时 | https://github.com/pollen-robotics/microduck | Apache-2.0 |
| 官方训练栈 + 仿真模型 | https://github.com/pollen-robotics/microduck_rl | Apache-2.0；MJCF + STL |
| **RPI Robot HAT（已开源）** | https://github.com/pollen-robotics/elec_RPI_Robot_HAT | **官方唯一完整开源的板件** |
| KiCAD 元件库 | https://github.com/pollen-robotics/lib_KiCAD | |
| 舵机库 | https://github.com/pollen-robotics/rustypot | 官方 Rust 舵机通信库 |
| 产品页 | https://pollen-robotics.com/microduck/ | $399 预售 |
| 发布博客 | https://pollen-robotics.com/microduck/blog/introducing-microduck/ | 官方规格 |

**引用官方源码片段**（注释、常量、寄存器定义）遵循其 **Apache-2.0** 许可。

---

## 三、厂商文档（走路线 B 必读）

| 来源 | 地址 |
|---|---|
| 飞特舵机官方文档中心 | https://doc.feetech.cn/ |
| 《SCS 通信协议》v1.0（2026-06-09） | 飞特官方发布 |
| 《磁编码 STS 内存表手册》v1.1（2026-08-27） | 飞特官方发布 |
| 官方 SDK `FTServo_Linux` | 飞特官方发布 |

**⚠️ 本仓库不包含飞特厂商文档副本**（版权归飞特所有）。
请从上述官方渠道或 `fanhao375/microduck-replica` 的 `docs/飞特资料/` 获取。

---

## 四、生态与工具

| 来源 | 地址 | 说明 |
|---|---|---|
| awesome-microduck | https://github.com/joeynyc/awesome-microduck | 生态资源汇总 |
| microduck-lab | https://github.com/jonathanhawkins/microduck-lab | |
| MicroDuckModels | https://github.com/IronSpiderMan/MicroDuckModels | Three.js 网页 3D 预览 |
| microduck-diy | https://github.com/ScrapMeta/microduck-diy | 「一个月手搓挑战」记录 |
| isaaclab-microduck | https://github.com/kabilankb/isaaclab-microduck | Isaac Lab 适配 |
| quackd | https://github.com/rokbenko/quackd | |
| microduck-gst-plugins | https://github.com/pollen-robotics/microduck-gst-plugins | 官方 GStreamer 插件 |

---

## 五、媒体报道与背景

| 来源 | 地址 | 用于 |
|---|---|---|
| CNX Software | 官方开源立场报道 | 「官方拒绝开源硬件标签」这一引述 |
| TechCrunch（2026-08-27） | https://techcrunch.com/2026/08/27/hugging-face-is-selling-a-cute-399-open-source-duck-robot-microduck/ | 产品发布、背景 |
| Engadget | https://www.engadget.com/2245407/ | 规格（32GB/1GB/2600mAh） |
| Hugging Face 博客（2025-04-14） | https://huggingface.co/blog/hugging-face-pollen-robotics-acquisition | **收购事实** |
| Pollen Robotics 官方博客 | 产品发布公告 | 规格与定位 |

---

## 六、本仓库做了什么

为免混淆，明确区分：

| 本仓库**没有**做的 | 本仓库**做了**的 |
|---|---|
| ❌ 实物制造与测试 | ✅ 多源检索与收集 |
| ❌ 参数实测 | ✅ 交叉核对与冲突裁决 |
| ❌ 价格核实 | ✅ 结构化整理为可执行清单 |
| ❌ 独家发现 | ✅ 错误说法的溯源与纠正 |
| ❌ 原始创作 | ✅ 补充缺失的上下文与决策依据 |

**唯一的原创贡献**是整理与核对工作，以及：

- 指出「HD-1910 协议兼容」是错误说法，并用厂商官方手册给出裁决依据
- 指出纸面估算与真实账单（¥11,923.60）之间存在约 2 倍的差距
- 把「来源冲突」显式记录为可验证条目，而不是擅自裁决

---

## 七、许可与署名要求

本仓库作为上述 **CC BY-NC-SA 4.0** 内容的衍生作品，同样以
**CC BY-NC-SA 4.0** 发布。

二次分发请保留：

1. 本文件（`SOURCES.md`）的完整署名
2. 对 **fanhao375**、**机械行者Robo**、**SaberOnGo**、**Fandcode** 的署名
3. 对 **Pollen Robotics** 及 **Hugging Face** 的产品与商标归属说明
4. **非商业性**与**相同方式共享**条款

### 与官方的隶属关系

**无。** 本仓库是独立的第三方资料整理，与 Pollen Robotics、Hugging Face
无任何隶属关系，也未获其背书。
