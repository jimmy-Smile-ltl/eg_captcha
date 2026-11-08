# Google reCAPTCHA — 样本、方法与求解器状态

> 厂商：Google reCAPTCHA | 来源：https://www.google.com/sorry/index | 类型：4 | 样本：36
>
> grid-independent/grid-split 仅收样本；audio 12 组保留 ASR 转写与 `result.json`，待人工审查；v3 token 不收录。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 思路与说明 |
|---|---|---|---:|---|---|---|---|
| `grid-independent` | 独立子图九宫格 | independent | 12 | VLM | 有求解器（exists）· 高 | pending | 视觉 LLM 直接读格；无需训练；旧批次 33/33 离线、7/7 线上；新批次待补跑 |
| `grid-split` | 整图切分九宫格 | split | 12 | VLM | 有求解器（exists）· 高 | pending | 视觉 LLM 直接读格；无需训练；旧批次 33/33 离线、7/7 线上；新批次待补跑 |
| `audio` | 音频听写 | recaptcha-v2-audio | 12 | ASR | 有求解器（exists）· 高 | pending | faster-whisper small.en；待人工听音频；源记录 20/20 离线、15/15 线上；待审 |
| `token` | 指纹 / Token 挑战 | v3-score | 0 | PROTO | 无求解器（none）· 未知 | not-applicable | v3 无题面；token 一次性、仅服务端评分；不收录 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。VLM 无需训练数据、按调用计费，起步便宜；量大时 CV 更便宜，常见组合是 CV/OCR 先定位、VLM 只对分歧或语义点调用。

## 二、样本速览

每类取 1 组代表样本；完整样本见 `cases/recaptcha/`。

| 类型 | 方法 | 代表样本 | 样本数 |
|---|---|---|---:|
| `grid-independent` | VLM | <img src="../cases/recaptcha/grid-independent/001/bg.jpg" width="180" alt="Google reCAPTCHA 独立子图九宫格 代表样本"> | 12 |
| `grid-split` | VLM | <img src="../cases/recaptcha/grid-split/001/bg.jpg" width="180" alt="Google reCAPTCHA 整图切分九宫格 代表样本"> | 12 |
| `audio` | ASR | 音频/无图（见案例目录） | 12 |
| `token` | PROTO | — | 0 |

### 音频题与 v3 说明

- audio：源 20 组中收录 12 组 MP3；Whisper small.en 转写，源记录离线 20/20、线上 15/15；单题待人工听音频。
- token（v3）：无题面，token 一次性、约 2 分钟过期且含敏感信息，不收录；只记录协议形态（前缀 `0cAF…`、约 2.4–2.6KB）。分数只在站点用 secret 调 `siteverify` 时可见。
- 图片题：源现有 independent 78 组 / split 35 组；本库按 12 组/类型精选；旧批次 33/33 离线、7/7 线上，新批次待补跑。

## 三、求解器与人工审查

> grid-independent/grid-split 仅收样本；audio 12 组保留 ASR 转写与 `result.json`，待人工审查；v3 token 不收录。 审查通过/不通过由维护者后续回填。

### 无求解器（none）

以下类型没有撰写求解器，仅为样本收录。原因包括未尝试、仅采集、优先级低、能力不足或未完成；具体以说明为准。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `token` | 0 | PROTO | 未知 | v3 无题面；token 一次性、仅服务端评分；不收录 |

### 有求解器、待人工审查（exists）

以下类型有求解器与源记录准确率，但本库未复制结果（或仅复制了明确标注的结果），等待人工审查。

| 类型 | 样本 | 方法 | 准确率（源记录） | 说明 |
|---|---:|---|---|---|
| `grid-independent` | 12 | VLM | 高 | 视觉 LLM 直接读格；无需训练；旧批次 33/33 离线、7/7 线上；新批次待补跑 |
| `grid-split` | 12 | VLM | 高 | 视觉 LLM 直接读格；无需训练；旧批次 33/33 离线、7/7 线上；新批次待补跑 |
| `audio` | 12 | ASR | 高 | faster-whisper small.en；待人工听音频；源记录 20/20 离线、15/15 线上；待审 |

## 四、来源

来源：Google reCAPTCHA Enterprise 图片题（/sorry 公开页）与 cap.guru 的 reCAPTCHA demo（音频 / v3，2026-11-07）。样本版权归 Google 所有；v3 token 不收录。
