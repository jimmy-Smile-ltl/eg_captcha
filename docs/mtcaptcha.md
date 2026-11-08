# MTCaptcha — 样本、方法与求解器状态

> 厂商：MTCaptcha | 来源：https://service.mtcaptcha.com/mtcv1/demo/ | 类型：1 | 样本：12
>
> 本页只记录求解器状态，不复制源求解器结果；`review.state` 初始为 `pending` 或 `not-applicable`，审查结果留空待回填。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 思路与说明 |
|---|---|---|---:|---|---|---|---|
| `text` | 字符识别 | standard | 12 | ASR+OCR+VLM | 有求解器（exists）· 中 | pending | Whisper 为主，ddddocr 兜底，VLM 仲裁；有标签子集 12/12；整体弱标签，待审 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。VLM 无需训练数据、按调用计费，起步便宜；量大时 CV 更便宜，常见组合是 CV/OCR 先定位、VLM 只对分歧或语义点调用。

## 二、样本速览

每类取 1 组代表样本；完整样本见 `cases/mtcaptcha/`。

| 类型 | 方法 | 代表样本 | 样本数 |
|---|---|---|---:|
| `text` | ASR+OCR+VLM | <img src="../cases/mtcaptcha/text/001/bg.gif" width="180" alt="MTCaptcha 字符识别 代表样本"> | 12 |

## 三、求解器与人工审查

> 本库只记录求解器状态，不复制源求解器结果。 审查通过/不通过由维护者后续回填。

### 有求解器、待人工审查（exists）

以下类型有求解器与源记录准确率，但本库未复制结果（或仅复制了明确标注的结果），等待人工审查。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `text` | 12 | ASR+OCR+VLM | 中 | Whisper 为主，ddddocr 兜底，VLM 仲裁；有标签子集 12/12；整体弱标签，待审 |

## 四、来源

来源：https://service.mtcaptcha.com/mtcv1/demo/。样本采集日期见各案例 `meta.json`；样本版权归厂商所有，仅用于研究与教学展示。
