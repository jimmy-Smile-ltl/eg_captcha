# 企讯通 — 样本、方法与求解器状态

> 厂商：企讯通 | 来源：http://www.qingtongbu.com/#experience | 类型：7 | 样本：84
>
> 本页只记录求解器状态，不复制源求解器结果；`review.state` 初始为 `pending` 或 `not-applicable`，审查结果留空待回填。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 思路与说明 |
|---|---|---|---:|---|---|---|---|
| `slide` | 滑动拼图 | classic | 12 | CV | 无求解器（none）· 未知 | not-applicable | 传统视觉：边缘/模板匹配；无 LLM，单次成本最低 |
| `text-click` | 文字点选 | classic | 12 | NONE(DET+OCR+VLM) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 YOLO+OCR+VLM 仲裁 |
| `icon-click` | 图标点选 | classic | 12 | NONE(VLM) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 VLM 读图 + 坐标点击 |
| `angle` | 角度验证 | classic | 12 | CV | 效果差（weak）· 低 | pending | 角度估计；MAE 5.56°，通过率 73%；MAE 5.56°，通过率 73.33%；参考价值有限 |
| `rotate` | 旋转验证 | classic | 12 | NONE(CV+VLM) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 CV 角度估计 + VLM 兜底 |
| `curve` | 曲线滑动 | classic | 12 | NONE(VLM+ACT) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 VLM 理解曲线 + 轨迹 |
| `random` | 智能随机 | classic | 12 | NONE | 无求解器（none）· 未知 | not-applicable | 随机路由，无独立题面 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。VLM 无需训练数据、按调用计费，起步便宜；量大时 CV 更便宜，常见组合是 CV/OCR 先定位、VLM 只对分歧或语义点调用。

## 二、样本速览

每类取 1 组代表样本；完整样本见 `cases/qingtongbu/`。

| 类型 | 方法 | 代表样本 | 样本数 |
|---|---|---|---:|
| `slide` | CV | <img src="../cases/qingtongbu/slide/001/bg.jpg" width="180" alt="企讯通 滑动拼图 代表样本"> | 12 |
| `text-click` | NONE(DET+OCR+VLM) | <img src="../cases/qingtongbu/text-click/001/bg.jpg" width="180" alt="企讯通 文字点选 代表样本"> | 12 |
| `icon-click` | NONE(VLM) | <img src="../cases/qingtongbu/icon-click/001/bg.jpg" width="180" alt="企讯通 图标点选 代表样本"> | 12 |
| `angle` | CV | <img src="../cases/qingtongbu/angle/001/bg.jpg" width="180" alt="企讯通 角度验证 代表样本"> | 12 |
| `rotate` | NONE(CV+VLM) | <img src="../cases/qingtongbu/rotate/001/bg.jpg" width="180" alt="企讯通 旋转验证 代表样本"> | 12 |
| `curve` | NONE(VLM+ACT) | <img src="../cases/qingtongbu/curve/001/bg.jpg" width="180" alt="企讯通 曲线滑动 代表样本"> | 12 |
| `random` | NONE | <img src="../cases/qingtongbu/random/001/bg.jpg" width="180" alt="企讯通 智能随机 代表样本"> | 12 |

## 三、求解器与人工审查

> 本库只记录求解器状态，不复制源求解器结果。 审查通过/不通过由维护者后续回填。

### 无求解器（none）

以下类型没有撰写求解器，仅为样本收录。原因包括未尝试、仅采集、优先级低、能力不足或未完成；具体以说明为准。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `slide` | 12 | CV | 未知 | 传统视觉：边缘/模板匹配；无 LLM，单次成本最低 |
| `text-click` | 12 | NONE(DET+OCR+VLM) | 未知 | 未撰写；建议 YOLO+OCR+VLM 仲裁 |
| `icon-click` | 12 | NONE(VLM) | 未知 | 未撰写；建议 VLM 读图 + 坐标点击 |
| `rotate` | 12 | NONE(CV+VLM) | 未知 | 未撰写；建议 CV 角度估计 + VLM 兜底 |
| `curve` | 12 | NONE(VLM+ACT) | 未知 | 未撰写；建议 VLM 理解曲线 + 轨迹 |
| `random` | 12 | NONE | 未知 | 随机路由，无独立题面 |

### 求解器效果差 / 参考价值有限（weak）

以下类型有求解器，但准确率低、误报多或只对特定样本有效；结果不宜当作可用能力。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `angle` | 12 | CV | 低 | 角度估计；MAE 5.56°，通过率 73%；MAE 5.56°，通过率 73.33%；参考价值有限 |

## 四、来源

来源：http://www.qingtongbu.com/#experience。样本采集日期见各案例 `meta.json`；样本版权归厂商所有，仅用于研究与教学展示。
