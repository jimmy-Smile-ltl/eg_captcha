# hCaptcha — 厂商索引

> 厂商：hCaptcha | 来源：<https://accounts.hcaptcha.com/demo> | 3 个类型（均有案例）| 36 组案例
>
> 交互式语义验证码厂商，题型覆盖整图区域点选、九宫格二分类与拖拽拼块。本库只做题面覆盖，未撰写求解器；建议路线统一是 **VLM 读题 + 结构化输出**，适合语义长尾、目标数量不固定的题。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细内容在类型页（下方表内链接）。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `area-select` | 区域点选 | 11 个题面族 | 12 | NONE(VLM) | 无求解器（none）· 未知 | not-applicable 12 | 整页指令 + 点选全部目标；VLM 读图找区域。[类型页](../cases/hcaptcha/area-select/README.md) |
| `binary` | 九宫格二分类 | 6 个题面族 | 12 | NONE(VLM) | 无求解器（none）· 未知 | not-applicable 12 | 3×3 网格逐格判断符合/不符合；VLM 读格。[类型页](../cases/hcaptcha/binary/README.md) |
| `drag-drop` | 拖拽拼块 | 5 个题面族 | 12 | NONE(VLM) | 无求解器（none）· 未知 | not-applicable 12 | 候选块拖到匹配空位；VLM/形状匹配 + 拖拽。[类型页](../cases/hcaptcha/drag-drop/README.md) |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**，不代表本库已复核；审查统计按案例 `meta.review` 现算（截至 2026-11-21）。
>
> 题面族共 22 个（area-select 11、binary 6、drag-drop 5），完整清单在各类型页；本索引不重复展开。

## 二、方法重点

- **VLM 是统一入口**：题面是英文关系判断，目标不是固定类别；先读题再定位，比先训检测器更省。
- **三类答案不同**：`area-select` 输出点坐标、`binary` 输出格子编号、`drag-drop` 输出落点并执行拖拽；后处理不能共用。
- **数量约束**：`binary` 逐格 yes/no，`area-select` 有「全部」「THREE」等数量词，`drag-drop` 有 1–3 个候选块；数量错误是最常见的失败形态。
- **没有本库指标**：36 组全部 `sample-only`、`review=not-applicable`；没有服务端答案或人工标注，不能把任何模型输出写成准确率。
- **证据边界**：本库只保存题面，不保存提交闭环；VLM 输出只能作为建议路线。

## 三、类型页导读

| 类型 | 类型页重点 |
|---|---|
| `area-select` | 11 个题面族、整页图与交互卡片的坐标差异、时序题（移动动物）的边界 |
| `binary` | 6 个题面族、3×3 拆格、逐格判断与勾选态的区分 |
| `drag-drop` | 5 个题面族、候选块/空位识别、单块与多块、拖拽动作未记录 |

## 四、代表样本

每类取 1 组，点击「详情」进入类型页；完整样本见 `cases/hcaptcha/`。

| 类型 | 方法 | 代表样本 | 详情 |
|---|---|---|---|
| `area-select` | NONE(VLM) | <img src="../cases/hcaptcha/area-select/001/bg.png" width="200" alt="hCaptcha 区域点选 001"> | [详情](../cases/hcaptcha/area-select/README.md) |
| `binary` | NONE(VLM) | <img src="../cases/hcaptcha/binary/001/bg.png" width="150" alt="hCaptcha 九宫格二分类 001"> | [详情](../cases/hcaptcha/binary/README.md) |
| `drag-drop` | NONE(VLM) | <img src="../cases/hcaptcha/drag-drop/001/bg.png" width="200" alt="hCaptcha 拖拽拼块 001"> | [详情](../cases/hcaptcha/drag-drop/README.md) |

## 五、厂商级结论

- **强项是语义**：hCaptcha 的题面多为开放关系判断（「这个栖息地的动物」「符合示例的运动方式」「破坏规律的形状」），不是固定类别表；VLM 是最省的统一路线。
- **交互类型三种**：整图点选、九宫格逐格二分类、拖拽拼块；三类的答案格式分别是 `points` / `indices` / `points`。
- **形态差异大**：同一 prompt 族下的图像分布也不同；整页图与交互卡片的坐标体系不同，读题必须用带指令的卡片。
- **难点**：开放语义、目标数可变、时序题（移动动物）、拖拽动作未记录；这四类问题都不是单一 CV 模型能覆盖的。
- **证据状态**：22 个题面族只有 12–12–12 组样本，不代表完整题面空间；不要用样本数推断成功率。

## 六、审查与证据

| 类型 | 样本 | 求解器 | 审查 | 证据等级 |
|---|---:|---|---|---|
| `area-select` | 12 | none · 未知 | not-applicable 12 | sample-only |
| `binary` | 12 | none · 未知 | not-applicable 12 | sample-only |
| `drag-drop` | 12 | none · 未知 | not-applicable 12 | sample-only |

> 36 组都没有求解结果，`review` 全部为 `not-applicable`；没有逐组真值，也没有服务端反馈。类型页第 6 节只解释「为什么没有结果」，不写准确率。

## 七、使用建议

- 先读类型页的题面表与样本图，再决定是否需要 VLM；不要直接从索引推断解法。
- 需要复现时先看类型页第 5 节的建议路线与第 7 节边界；索引只做导航。
- 引用数据时以 `meta.json` 为准；本索引的统计截至 2026-11-21，案例状态可能随后续批次变化。
- 本库不保存可执行求解器；类型页里的 `text` 伪代码只是路线说明，不是可直接运行的脚本。

## 八、来源

来源：<https://accounts.hcaptcha.com/demo>。样本采集日期见各案例 `meta.json`；样本版权归 hCaptcha 所有，仅用于研究与教学展示。
