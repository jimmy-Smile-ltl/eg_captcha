# 数美 — 样本、方法与求解器状态

> 厂商：数美 | 来源：https://www.ishumei.com/new/product/tw/code | 类型：5 | 样本：50
>
> 本页只记录求解器状态，不复制源求解器结果；`review.state` 初始为 `pending` 或 `not-applicable`，审查结果留空待回填。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 思路与说明 |
|---|---|---|---:|---|---|---|---|
| `slide` | 滑动拼图 | classic | 10 | CV+ACT | 效果差（weak）· 低 | pending | CV 定位缺口 + 轨迹；服务端轨迹 ML 拒绝，效果差；服务端轨迹 ML 100% 拒绝模拟轨迹；图像答案无意义 |
| `text-click` | 文字点选 | classic | 10 | DET+OCR+VLM | 效果差（weak）· 低 | pending | YOLO + OCR/Siamese + LLM 版；风格化字体导致效果差；风格化字体 OCR 0%，bbox 偏差大；参考价值有限 |
| `icon-click` | 图标点选 | classic | 10 | CV | 有求解器（exists）· 中 | pending | 红色像素 + 多角度 NCC + 匈牙利；无 LLM；源方案 10/10，置信度 0.679；待审 |
| `word-order` | 语序点选 | classic | 10 | NONE(OCR+VLM) | 无求解器（none）· 未知 | not-applicable | 未撰写；建议 OCR + VLM 语序 |
| `spatial` | 空间推理 | classic | 10 | CV | 有求解器（exists）· 高 | pending | 颜色/形状规则 + 面积极值；无 LLM；源记录 2/2；结果待审查 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。VLM 无需训练数据、按调用计费，起步便宜；量大时 CV 更便宜，常见组合是 CV/OCR 先定位、VLM 只对分歧或语义点调用。

## 二、样本速览

每类取 1 组代表样本；完整样本见 `cases/shumei/`。

| 类型 | 方法 | 代表样本 | 样本数 |
|---|---|---|---:|
| `slide` | CV+ACT | <img src="../cases/shumei/slide/001/bg.jpg" width="180" alt="数美 滑动拼图 代表样本"> | 10 |
| `text-click` | DET+OCR+VLM | <img src="../cases/shumei/text-click/001/bg.jpg" width="180" alt="数美 文字点选 代表样本"> | 10 |
| `icon-click` | CV | <img src="../cases/shumei/icon-click/001/bg.jpg" width="180" alt="数美 图标点选 代表样本"> | 10 |
| `word-order` | NONE(OCR+VLM) | <img src="../cases/shumei/word-order/001/bg.jpg" width="180" alt="数美 语序点选 代表样本"> | 10 |
| `spatial` | CV | <img src="../cases/shumei/spatial/001/bg.jpg" width="180" alt="数美 空间推理 代表样本"> | 10 |

## 三、求解器与人工审查

> 本库只记录求解器状态，不复制源求解器结果。 审查通过/不通过由维护者后续回填。

### 无求解器（none）

以下类型没有撰写求解器，仅为样本收录。原因包括未尝试、仅采集、优先级低、能力不足或未完成；具体以说明为准。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `word-order` | 10 | NONE(OCR+VLM) | 未知 | 未撰写；建议 OCR + VLM 语序 |

### 求解器效果差 / 参考价值有限（weak）

以下类型有求解器，但准确率低、误报多或只对特定样本有效；结果不宜当作可用能力。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `slide` | 10 | CV+ACT | 低 | CV 定位缺口 + 轨迹；服务端轨迹 ML 拒绝，效果差；服务端轨迹 ML 100% 拒绝模拟轨迹；图像答案无意义 |
| `text-click` | 10 | DET+OCR+VLM | 低 | YOLO + OCR/Siamese + LLM 版；风格化字体导致效果差；风格化字体 OCR 0%，bbox 偏差大；参考价值有限 |

### 有求解器、待人工审查（exists）

以下类型有求解器与源记录准确率，但本库未复制结果（或仅复制了明确标注的结果），等待人工审查。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `icon-click` | 10 | CV | 中 | 红色像素 + 多角度 NCC + 匈牙利；无 LLM；源方案 10/10，置信度 0.679；待审 |
| `spatial` | 10 | CV | 高 | 颜色/形状规则 + 面积极值；无 LLM；源记录 2/2；结果待审查 |

## 四、来源

来源：https://www.ishumei.com/new/product/tw/code。样本采集日期见各案例 `meta.json`；样本版权归厂商所有，仅用于研究与教学展示。
