# Google reCAPTCHA · 音频听写（audio）

> 一段约 4–5 秒的英文短句，听写后填入 `#audio-response`；用 faster-whisper small.en 整段转写，源记录 20/20 离线、15/15 线上。
>
> 返回：[reCAPTCHA 索引](../../../docs/recaptcha.md) · [全量类型](../../../docs/types.md) · [方法论](../../../docs/00-methodology.md)

## 一、速览

| 项 | 值 |
|---|---|
| 厂商 | Google reCAPTCHA（`recaptcha`） |
| 类型 / 变体 | `audio` / recaptcha-v2-audio |
| 中文 | 音频听写 |
| 媒体 | 音频（12 条 `audio.mp3`，无题面图） |
| 样本 | 12 组（001–012） |
| 答案格式 | `text`：整段英文短句 |
| 方法 | `ASR`（faster-whisper small.en） |
| 求解器 | `exists` · 高（源记录 20/20 离线、15/15 线上） |
| 人工审查 | 12 通过 / 0 错误（截至 2026-11-21） |
| 证据等级 | `unverified`（转写结果，未在本库闭环提交） |

## 二、题面与判定

题面固定为英文 `Press PLAY to listen / Enter what you hear`：点播放，听完后把听到的整段英文填入输入框。12 组音频互不重复，答案空间是开放文本，不是选项或格子编号。

| 案例 | 题面（`meta.prompt`） | 通过条件 | 转写（`answer.txt`） |
|---|---|---|---|
| [001](001/) | Press PLAY to listen / Enter what you hear | 该批案例未记录 `pass_criteria`；题面要求提交听到的内容，由服务端判定 | Make smart, informed decisions. |
| [002](002/) | 同上 | 同上 | Straight lines and... |
| [003](003/) | 同上 | 同上 | Cations and styles |
| [008](008/) | 同上 | 同上 | Give them instructions. |

> 12 组题面完全一致，差异只在音频内容。`answer.txt` 保留 Whisper 原文，包括边缘省略号和尾逗号；本库没有官方逐字稿。

## 三、样本长什么样

本类型只有音频，没有 `bg` / `annotated` / `solved` 图片。音频是唯一题面，点击链接即可试听；转写与时间戳在 `result.json`。

| 案例 | 音频 | 案例 | 音频 | 案例 | 音频 |
|---|---|---|---|---|---|
| [001](001/) | [听音频 001](001/audio.mp3) | [002](002/) | [听音频 002](002/audio.mp3) | [003](003/) | [听音频 003](003/audio.mp3) |
| [004](004/) | [听音频 004](004/audio.mp3) | [005](005/) | [听音频 005](005/audio.mp3) | [006](006/) | [听音频 006](006/audio.mp3) |
| [007](007/) | [听音频 007](007/audio.mp3) | [008](008/) | [听音频 008](008/audio.mp3) | [009](009/) | [听音频 009](009/audio.mp3) |
| [010](010/) | [听音频 010](010/audio.mp3) | [011](011/) | [听音频 011](011/audio.mp3) | [012](012/) | [听音频 012](012/audio.mp3) |

- 12 条 MP3 时长 4.10–5.16 秒，都是单段英文短句，`result.json` 没有多说话人字段。
- `result.json` 的 `words` 给出每个词/短语的起止时间，可用于核对首尾静音与漏词；`answer.txt` 是可直接提交的整段文本。

## 四、形态与难点

- **短句听写**：一句话 4–5 秒，内容可能是短语、半句或带停顿的片段，不一定语法完整。
- **开放文本**：没有候选词表，必须逐词听对；大小写、标点、空格都影响提交文本。
- **边缘标点**：Whisper 会在句首/句尾输出 `...`，也会保留句尾逗号；这些不是额外的词，但会进入 `answer.txt`。
- **单模型风险**：`method_note` 写明使用 faster-whisper small.en；小模型对专名、连读更敏感，边缘样本需要更大模型复核。

## 五、求解思路

源记录是一条标准 ASR 管线：解码音频、整段转写、按时间戳拼接、直接作为 `#audio-response` 提交。`method_note` 只记录 small.en，本批没有更大模型的复核记录。

| 步骤 | 做法 | 关键判据 / 参数 | 12 组结果 |
|---|---|---|---|
| 1 解码 | 读取 `audio.mp3` | 单声道短音频 | 12 条均可解码 |
| 2 转写 | faster-whisper `small.en` 整段识别 | `language_probability=1`；单题约 3.5–3.9 秒 | 12 组均给出英文文本 |
| 3 拼接 | 按时间戳顺序拼成整句 | 保留 ASR 标点与省略号；`words` 为 `[词, 起, 止]` | 2–4 段/组；12 组 `answer.txt` |
| 4 提交 | 填入 `#audio-response` | 文本原样提交 | 源记录线上 15/15 |

```text
# 音频听写（伪代码，对应源方案步骤）

audio = decode("audio.mp3")
result = faster_whisper(audio, model="small.en", language="en")
text = join(segment.text for segment in result.segments)   # 保留原始标点
submit("#audio-response", text)
```

## 六、结果与审查

- **源记录**：20/20 离线、15/15 线上（`meta.solver.notes`）；这是源求解器的历史成绩，不是本库 12 组的逐题成绩。
- **本库结果**：12 组都有 `audio.mp3` / `answer.txt` / `result.json`；`result.json` 含 `transcript`、词级时间戳、时长、模型与推理耗时。
- **人工审查**：12 组 `review.state=pass`、`result=correct`；审查方式是听音频核对转写，不等同线上提交。
- **证据等级**：12 组 `evidence=unverified`；没有服务端返回的官方文本，`answer.txt` 是 ASR 建议提交。

| 组 | 转写（`answer.txt`） | 时长 (s) | 段数 | 审查 |
|---|---|---:|---:|---|
| [001](001/) | Make smart, informed decisions. | 4.42 | 4 | 通过 |
| [002](002/) | Straight lines and... | 4.63 | 3 | 通过 |
| [003](003/) | Cations and styles | 4.44 | 3 | 通过 |
| [004](004/) | Advanced environment. | 4.91 | 2 | 通过 |
| [005](005/) | power windows, electronics, | 5.16 | 3 | 通过 |
| [006](006/) | A weekday, a weekender. | 4.30 | 4 | 通过 |
| [007](007/) | Just frame, this behavior | 4.39 | 4 | 通过 |
| [008](008/) | Give them instructions. | 4.10 | 3 | 通过 |
| [009](009/) | ...molding natural gas... | 4.66 | 3 | 通过 |
| [010](010/) | Doing the enchiladas. | 4.20 | 3 | 通过 |
| [011](011/) | Please, please, please subscribe. | 4.32 | 4 | 通过 |
| [012](012/) | Convertible is ready. | 4.21 | 3 | 通过 |

> 时长与段数按 `result.json` 的 `duration` / `words` 现算；转写保留原文标点，未做清洗。

## 七、失败与边界

| 现象 | 根因 | 证据 | 结论 |
|---|---|---|---|
| 002 结尾出现 `...` | ASR 对句尾未识别片段输出省略号；是静音还是漏词未核实 | `words` 最后词止于 3.50 秒，音频长 4.63 秒；`answer.txt` 注明待人工听音 | 原样保留，不补词 |
| 009 首尾都有 `...` | ASR 原文带首尾省略号，本库未做去静音核验 | `answer.txt` 为 `...molding natural gas...`；首词时间戳从 0 开始 | 原样保留，不当作缺字 |
| 005 结尾有逗号 | ASR 保留了句中标点/停顿 | `answer.txt` 为 `power windows, electronics,` | 不擅自删标点；提交文本以原文为准 |
| 单模型可能听错专名/连读 | small.en 容量有限 | `method_note` 只记录 small.en，本批无更大模型复核记录 | 边缘样本建议先用更大模型复核，再回填 |
| 无法离线算准确率 | 没有官方逐字稿 | 12 组只有 ASR 结果与人工听音审查 | 源记录只作参考，不写成官方通过率 |

**边界**：本页不给「逐题正确率」。12 组审查通过只表示人工听音认为转写合理，不表示服务端一定接受；真实提交还需要正确的 `#audio-response` 表单与会话状态。

## 八、相关

- 厂商索引：[docs/recaptcha.md](../../../docs/recaptcha.md)
- 全量类型：[docs/types.md](../../../docs/types.md) · 方法论：[docs/00-methodology.md](../../../docs/00-methodology.md)
- 同厂商对照：[grid-independent](../grid-independent/)（3×3 独立子图）、[grid-split](../grid-split/)（4×4 整图切分）
- 案例：[001](001/) – [012](012/)（每组含 `meta.json` / `audio.mp3` / `answer.txt` / `result.json`）
- 来源：cap.guru reCAPTCHA demo <https://test.cap.guru/demo/recap#recap2>，采集于 2026-11-07；样本版权归 Google 所有，仅用于研究与教学。
