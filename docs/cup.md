# 中国银联 — 厂商索引

> 厂商：中国银联（`cup`）| 来源：<https://captcha.cup.com.cn/> | 5 个类型（4 个有案例、1 个仅文档）| 48 组案例
>
> 银联体验站的题型偏小图与语义点选：300×150 滑动拼图、300×150 文字点选、300×300 旋转回正、3×2 场景语义多选。共同点是**案例只有图片产物，没有 `answer.txt` / `result.json`**，审查只能看叠加图/标注，无法从数据层复算准确率。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细内容在类型页（下方第 8 列）。

## 一、厂商概览

银联的 5 个类型可以分成三组：

- **一维定位组**：`slide` 与仅文档的 `slide-no-image`。前者是小图拼图缺口，后者没有题面媒体，靠逻辑/交互完成。
- **点选组**：`click` 与 `scene-click`。前者是 5 个彩色字里点 4 个提示字，后者是六格 AI 场景里选中所有符合描述的图。
- **旋转组**：`rotate` 把 300×300 圆形照片转正；源记录同时试过 CV 与 VLM，均未达可用标准。

这批案例没有线上提交记录，也没有机器可读答案；`review` 已在 `meta.json` 回填，但审查对象是图片标注，不是求解器输出。

## 二、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `slide` | 滑动拼图 | classic | 12 | CV | 有求解器（exists）· 高（源记录「已攻克」，无准确率数字） | 12 通过 | 300×150 + 70×50 拼图块；Canny + NCC；无 LLM；只有 `solved.png`，无坐标文件。[类型页](../cases/cup/slide/README.md) |
| `slide-no-image` | 无图滑块 | classic | 0 | PROTO/ACT | 有求解器（exists）· 高（源记录 15/15） | not-applicable | 无图滑块，靠逻辑/交互推进；无视觉模型、无题面媒体；仅文档收录，无案例目录与类型页。 |
| `click` | 通用点选 | classic | 12 | NONE(VLM) | 无求解器（none）· 未知 | 6 通过 / 5 部分 / 1 错误 | 300×150 场景 5 字 + 100×20 提示条 4 字；有 `annotated.png`，无 `answer/result`；审查的是标注。[类型页](../cases/cup/click/README.md) |
| `rotate` | 旋转验证 | classic | 12 | CV+VLM | 效果差（weak）· 低（源记录待优化） | 1 通过 / 5 错误 / 6 待审 | 300×300 圆形照片回正；CV 与 VLM 均未达可用标准；001–006 有 `solved.png`，007–012 只有 `bg.png`。[类型页](../cases/cup/rotate/README.md) |
| `scene-click` | 场景语义点选 | classic | 12 | VLM | 有求解器（exists）· 高（源 LLM；notes 写「已搁置」） | 12 通过 | 3×2 六格 AI 场景，按中文描述选中所有命中图；只有 `annotated.png`；审查的是红框/准星。[类型页](../cases/cup/scene-click/README.md) |

> 方法标签：`CV`=传统视觉；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`VLM`=多模态 LLM；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**；审查统计按案例 `meta.review` 现算（截至 2026-11-21）。本表保持 8 列，viewer 依赖第 4 列样本数与第 8 列链接。

## 三、代表样本

每类取 1 组；`slide-no-image` 没有媒体。完整样本见 `cases/cup/`。

| 类型 | 方法 | 代表样本 | 详情 |
|---|---|---|---|
| `slide` | CV | <img src="../cases/cup/slide/001/bg.png" width="150" alt="银联滑动拼图 001"> | [详情](../cases/cup/slide/README.md) |
| `click` | NONE(VLM) | <img src="../cases/cup/click/001/annotated.png" width="150" alt="银联通用点选 001 标注"> | [详情](../cases/cup/click/README.md) |
| `rotate` | CV+VLM | <img src="../cases/cup/rotate/001/solved.png" width="150" alt="银联旋转验证 001 回正"> | [详情](../cases/cup/rotate/README.md) |
| `scene-click` | VLM | <img src="../cases/cup/scene-click/010/annotated.png" width="150" alt="银联场景语义点选 010 标注"> | [详情](../cases/cup/scene-click/README.md) |

## 四、厂商级结论

- **强项**：`scene-click` 的 12 组标注全部通过，题面语义清楚、六格布局稳定；`slide` 的源记录标为「已攻克」，Canny + NCC 的思路与同类拼图一致。
- **弱点**：`rotate` 是明确短板——12 组只有 001 审查通过，002–006 错误，007–012 连结果图都没有；CV 与 VLM 两条路线都未达可用标准。
- **点选的真相**：`click` 没有求解器，6 通过 / 5 部分 / 1 错误是**标注审查**，不是模型准确率；010 的标注本身被标错，说明这批点选题连真值都需要复核。
- **语义题**：`scene-click` 的「所有」是实质要求，010 命中第 3、4 两格；只点一张会漏选。源 LLM 求解器虽标 `high`，但 notes 写「源记录已搁置」，本库没有它的结果文件。
- **证据边界**：4 个有案例的类型全部没有 `answer.txt` / `result.json`，`meta` 也没有 `evidence` 字段；只能看 `solved.png` / `annotated.png`，不能重算误差，也没有线上提交。
- **审查口径**：旧文档把多数类型写成 `pending`，但 `meta.review` 已回填；本索引按 meta 统计，并明确区分「求解准确率」与「标注审查」。
- **仅文档类型**：`slide-no-image` 样本为 0、无案例目录，保留在表中但不放链接；源记录 15/15 只是记录，不是本库复核。
- **与企讯通的差异**：企讯通同类案例有 `answer.txt` / `result.json` 和过程图，银联只有图片；因此银联的类型页更适合看题面形态，不适合比较求解精度。
- **成本**：`slide` 可纯 CPU；`click` / `scene-click` 建议 VLM，按调用计费；`rotate` 目前没有值得部署的路线。
- **样本用途**：48 组里 24 组是点选/语义题，12 组拼图，12 组旋转；适合做题型覆盖，不足以训练稳定模型。

## 五、相关索引

- 全量类型与厂商总览：[types.md](types.md) · [README](../README.md)
- 同平台对照（有机器可读结果）：[企讯通索引](qingtongbu.md)
- 方法论与成本：[00-methodology.md](00-methodology.md)

## 六、来源与版权

来源：<https://captcha.cup.com.cn/>。样本采集于 2026-07-23，每组源路径见案例 `meta.json`；版权归中国银联所有，仅用于研究与教学展示。统计截至 2026-11-21，以案例 `meta.json` 为准。
