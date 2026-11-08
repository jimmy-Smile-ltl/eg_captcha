# 中国银联 — 样本、方法与求解器状态

> 厂商：中国银联 | 来源：https://captcha.cup.com.cn/ | 类型：5 | 样本：48
>
> 本页只记录求解器状态，不复制源求解器结果；`review.state` 初始为 `pending` 或 `not-applicable`，审查结果留空待回填。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 思路与说明 |
|---|---|---|---:|---|---|---|---|
| `slide` | 滑动拼图 | classic | 12 | CV | 有求解器（exists）· 高 | pending | Canny + NCC；无 LLM；源记录已攻克；待审 |
| `slide-no-image` | 无图滑块 | classic | 0 | PROTO/ACT | 有求解器（exists）· 高 | pending | 无图滑块，逻辑/交互；无视觉模型；15/15；无题面媒体 |
| `rotate` | 旋转验证 | classic | 12 | CV+VLM | 效果差（weak）· 低 | pending | LLM 与 CV 均未达可用标准；源记录待优化；参考价值有限 |
| `click` | 通用点选 | classic | 12 | NONE(VLM) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 VLM 直接读图，起步便宜 |
| `scene-click` | 场景语义点选 | classic | 12 | VLM | 有求解器（exists）· 高 | pending | 视觉 LLM 场景语义点选；源 LLM 求解器；待审 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。VLM 无需训练数据、按调用计费，起步便宜；量大时 CV 更便宜，常见组合是 CV/OCR 先定位、VLM 只对分歧或语义点调用。

## 二、样本速览

每类取 1 组代表样本；完整样本见 `cases/cup/`。

| 类型 | 方法 | 代表样本 | 样本数 |
|---|---|---|---:|
| `slide` | CV | <img src="../cases/cup/slide/001/bg.png" width="180" alt="中国银联 滑动拼图 代表样本"> | 12 |
| `slide-no-image` | PROTO/ACT | — | 0 |
| `rotate` | CV+VLM | <img src="../cases/cup/rotate/001/bg.png" width="180" alt="中国银联 旋转验证 代表样本"> | 12 |
| `click` | NONE(VLM) | <img src="../cases/cup/click/001/bg.png" width="180" alt="中国银联 通用点选 代表样本"> | 12 |
| `scene-click` | VLM | <img src="../cases/cup/scene-click/001/bg.png" width="180" alt="中国银联 场景语义点选 代表样本"> | 12 |

## 三、求解器与人工审查

> 本库只记录求解器状态，不复制源求解器结果。 审查通过/不通过由维护者后续回填。

### 无求解器（none）

以下类型没有撰写求解器，仅为样本收录。原因包括未尝试、仅采集、优先级低、能力不足或未完成；具体以说明为准。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `click` | 12 | NONE(VLM) | 未知 | 未撰写；建议 VLM 直接读图，起步便宜 |

### 求解器效果差 / 参考价值有限（weak）

以下类型有求解器，但准确率低、误报多或只对特定样本有效；结果不宜当作可用能力。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `rotate` | 12 | CV+VLM | 低 | LLM 与 CV 均未达可用标准；源记录待优化；参考价值有限 |

### 有求解器、待人工审查（exists）

以下类型有求解器与源记录准确率，但本库未复制结果（或仅复制了明确标注的结果），等待人工审查。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `slide` | 12 | CV | 高 | Canny + NCC；无 LLM；源记录已攻克；待审 |
| `slide-no-image` | 0 | PROTO/ACT | 高 | 无图滑块，逻辑/交互；无视觉模型；15/15；无题面媒体 |
| `scene-click` | 12 | VLM | 高 | 视觉 LLM 场景语义点选；源 LLM 求解器；待审 |

## 四、来源

来源：https://captcha.cup.com.cn/。样本采集日期见各案例 `meta.json`；样本版权归厂商所有，仅用于研究与教学展示。
