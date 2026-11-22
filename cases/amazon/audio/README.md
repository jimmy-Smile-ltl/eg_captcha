# Amazon AWS WAF · 音频听写（audio）

> 一段双人混音：女声念「Type one of the two following words spoken by me」和两个单词，男声同时朗读文章；用 SepFormer 分离 + F0 定角色 + 三模型 ASR 融合，取出女声的两个词，任选其一提交。
>
> 返回：[Amazon 索引](../../../docs/amazon.md) · [全量类型](../../../docs/types.md) · [方法论](../../../docs/00-methodology.md)

## 一、速览

| 项 | 值 |
|---|---|
| 厂商 | Amazon AWS WAF（`amazon`） |
| 类型 / 变体 | `audio` / noisyneighbour-en-us |
| 中文 | 音频听写 |
| 媒体 | 音频（12 条 `audio.aac`；仅 001 附页面截图 `bg.png`） |
| 样本 | 12 组（001–012） |
| 答案格式 | `text`：女声所念两个英文单词中的任意一个 |
| 方法 | `ASR`（SepFormer 双人分离 + F0 判男女 + 三模型融合 + 原混音复核） |
| 求解器 | `exists` · 高（源记录 45/46 轮最终通过；61 次提交 45 成功） |
| 人工审查 | 11 通过 / 1 待审（002；截至 2026-11-21） |
| 证据等级 | `unverified`（离线 ASR 融合结果，未在本库闭环提交） |

## 二、题面与判定

题面在 12 组中完全一致：听一段约 10 秒的混音，输入女声（出题人）念的两个英文单词中的任意一个。两个声音的角色是固定的——女声出题、男声只是背景旁白（noisy neighbour），不是让用户在两个说话人之间做选择。

| 案例 | 题面（`meta.prompt`） | 通过条件 | 定稿两词（`result.json`） |
|---|---|---|---|
| [001](001/) | 听音频，输入女声（出题人）念的两个英文单词中的任意一个 | 该批案例未记录 `pass_criteria`；题面要求填入两词中的任意一个，最终由服务端判定 | double / stroke |
| [002](002/) | 同上 | 同上 | proceed / lifetime |
| [003](003/) | 同上 | 同上 | island / awareness |
| [008](008/) | 同上 | 同上 | existing / threat |

> 答案空间是开放的英文单词，不是固定选项；本库只保存 `result.json` 定稿的两个词，没有服务端真值。

## 三、样本长什么样

音频是主素材，12 组每组一条 AAC。图片只有 001 有：它是题面 widget 的截图（中文界面、播放按钮、输入框），不能替代音频。002–012 的 `meta.roles` 只有 `audio`。

| 案例 | 音频 | 案例 | 音频 |
|---|---|---|---|
| [001](001/) | [听音频 001](001/audio.aac) | [002](002/) | [听音频 002](002/audio.aac) |
| [003](003/) | [听音频 003](003/audio.aac) | [004](004/) | [听音频 004](004/audio.aac) |
| [005](005/) | [听音频 005](005/audio.aac) | [006](006/) | [听音频 006](006/audio.aac) |
| [007](007/) | [听音频 007](007/audio.aac) | [008](008/) | [听音频 008](008/audio.aac) |
| [009](009/) | [听音频 009](009/audio.aac) | [010](010/) | [听音频 010](010/audio.aac) |
| [011](011/) | [听音频 011](011/audio.aac) | [012](012/) | [听音频 012](012/audio.aac) |

| 001 的页面截图（唯一有 `bg` 的一组） |
|---|
| <img src="001/bg.png" width="240" alt="Amazon 音频听写 001 题面 widget 截图"> |

- 截图来自 320×430 的页面：标题「我们需要确认您是人类」、播放按钮、回答输入框和确认按钮。
- 音频本体比截图重要：两词、女声/男声角色、F0 与一致度都在 `result.json` 中，不在图片里。

## 四、形态与难点

- **双人同轨**：女声负责固定指令句和两个单词，男声从头到尾朗读文章。两轨重叠，直接整段转写会跟着旁白走。
- **角色固定**：12 组 `result.json` 的 `roles` 都是 female=questioner、male=narrator；女声才是答案来源。
- **F0 可分离**：源记录用 40 ms 自相关基频中值核对，女声约 156.9–250 Hz、男声约 101.3–114.3 Hz；12/12 与指令句所在轨一致。
- **时长稳定**：`audio_s` 为 9.98–10.24 秒，都是约 10 秒的 AAC；固定指令句是定位两词的时间锚点。
- **第二词易错**：分离轨在第二词上常出现音近幻觉，必须用原混音复核；这是本类型的主要难点。

## 五、求解思路

核心不是「把整段听清楚」，而是「先把女声分离出来，再用固定指令句定位两个词，最后让多个模型和原混音互相校验」。

| 步骤 | 做法 | 关键判据 / 参数 | 12 组结果 |
|---|---|---|---|
| 1 双人分离 | SepFormer 把混音拆成两条说话人轨，保留原混音 | 两人同时说话，旁白贯穿全程 | 12 组均得到两轨 |
| 2 角色判定 | 40 ms 自相关基频中值；女声=出题人、男声=旁白 | 女声 156.9–250 Hz、男声 101.3–114.3 Hz | 12/12 与指令句所在轨一致 |
| 3 多模型转写 | small.en / large-v3-turbo / distil-large-v3 分别转写女声轨与原混音 | 混音加固定指令提示词，避免整段跟着旁白走 | 每个模型保留原始转写在 `transcript_asr` |
| 4 定位两词 | 找到指令句，取其后头两个词 | 指令句固定，两词紧随其后 | 12 组均取到两词 |
| 5 融合定稿 | 按音近合并候选，少数服从多数，并用混音大模型复核 | 一致度高/中；分歧写进 `words_alt` | 12 组定稿；4 组标 medium |
| 6 输出 | 取两词之一作为建议提交，保留双人全文 | 任选其一即可；答案仍标 `unverified` | 12 组 `answer` 齐全 |

```text
# 双人混音听写（伪代码，对应源方案步骤）

tracks = sepformer(mix)                       # 两条说话人轨 + 原混音
female = track with median_f0 in 157..250 Hz  # 40 ms 自相关基频
male   = track with median_f0 in 101..114 Hz
# 角色固定：female = questioner，male = narrator

instr = "Type one of the two following words spoken by me."
for model in [small.en, large-v3-turbo, distil-large-v3]:
    hyp_female[model] = asr(model, female)
    hyp_mix[model]    = asr(model, mix, prompt=instr)   # 指令提示词锚定

words = first_two_words_after(instr, merge(hyp_female, hyp_mix))
answer = words[0] or words[1]                 # 任选其一提交
agreement = high if models_agree(words) else medium
```

> 融合不是简单投票：001 的分离轨出现 doubled/stokes、devil/stunk、dibble/stoke，最终靠混音大模型的 double/stroke 收敛；003、008 的第二词也是混音复核后才定稿。

## 六、结果与审查

- **源记录**：45/46 轮最终通过（61 次提交 45 成功）；这是源求解器的历史记录，不是本库 12 组的逐题成绩。
- **本库结果**：12 组都有 `answer.txt` / `result.json` / `audio.aac`；两个词、建议提交、一致度与双人转录都在 `result.json` 的 `speakers` / `words` / `answer` 字段。
- **人工审查**：11 组 `review.state=pass`、`result=correct`；002 仍 `pending`。审查是听音频核对两词，不等同线上提交。
- **证据等级**：12 组 `evidence=unverified`；音频本身是主证据，`result.json` 是离线 ASR 融合结果。

| 组 | 女声 F0 (Hz) | 男声 F0 (Hz) | 两个词 | 建议提交 | 一致度 | 审查 |
|---|---:|---:|---|---|---|---|
| [001](001/) | 205.1 | 108.1 | double / stroke | double | medium | 通过 |
| [002](002/) | 156.9 | 106.7 | proceed / lifetime | proceed | high | 待审 |
| [003](003/) | 177.9 | 109.6 | island / awareness | island | medium | 通过 |
| [004](004/) | 205.1 | 114.3 | channel / operator | channel | high | 通过 |
| [005](005/) | 156.9 | 106.0 | unless / despite | unless | high | 通过 |
| [006](006/) | 228.6 | 108.1 | aircraft / office | aircraft | high | 通过 |
| [007](007/) | 186.0 | 101.3 | himself / infant | himself | high | 通过 |
| [008](008/) | 205.1 | 105.3 | existing / threat | existing | medium | 通过 |
| [009](009/) | 200.0 | 103.9 | emerge / general | emerge | medium | 通过 |
| [010](010/) | 200.0 | 102.6 | economic / training | economic | high | 通过 |
| [011](011/) | 250.0 | 106.7 | freeze / opposite | freeze | high | 通过 |
| [012](012/) | 200.0 | 108.1 | stream / inform | stream | high | 通过 |

> F0 是 `result.json` 记录的 40 ms 自相关中值；女声最小 156.9、最大 250 Hz，男声最小 101.3、最大 114.3 Hz。两个词与一致度均按 `result.json` 摘录。

## 七、失败与边界

| 现象 | 根因 | 证据 | 结论 |
|---|---|---|---|
| 001 分离轨听成 doubled/stokes、devil/stunk、dibble/stoke | 分离后高频细节受损，单轨多模型各有异读 | 原混音 large 给出 double/stroke；distil 混音也落在 Double/Strokes 附近 | 以混音复核为准，定稿 double/stroke |
| 003 分离轨小模型反复输出 `I mean it` | 重复幻觉，不是三个词 | small.en 女声轨连续三次 `I mean it`；混音 large 明确 Island/Awareness（两词置信度 0.88/1.00） | 判为分离失真，取 island/awareness |
| 008 第二词在分离轨上变成 connect/smart/right | 第二词靠近句尾，分离轨信息不足 | 原混音 large 稳定 threat；高通 150/180 Hz、减旁白变体也一致；旁白 treats 在 6.92–7.36 s，女声第二词在 8.75–9.15 s | 排除旁白泄漏，定稿 threat |
| 011 small.en 女声轨听成 `Please` | 分离失真导致首音误判 | turbo/distil 与混音 large 均为 freeze/opposite | 按多模型多数定 freeze |
| 002 仍待审 | 人工听音复核未完成 | `review.state=pending` | 不把 pending 写成通过 |
| 源记录不是 46/46 | 有一轮最终未通过 | `solver.notes`：45/46 轮（61 提交 45 成功） | 源记录原样引用，不补成满分 |

**边界**：本页不给「线上通过率」。`answer` 是离线融合的建议提交，不是服务端返回的真值；分离、F0 与三模型只解决「听出两个词」，不涉及提交后的风控判定。

## 八、相关

- 厂商索引：[docs/amazon.md](../../../docs/amazon.md)
- 全量类型：[docs/types.md](../../../docs/types.md) · 方法论：[docs/00-methodology.md](../../../docs/00-methodology.md)
- 同厂商对照：[grid](../grid/)（九宫格多选，VLM）
- 案例：[001](001/) – [012](012/)（每组含 `meta.json` / `audio.aac` / `answer.txt` / `result.json`；001 另有 `bg.png`）
- 来源：wuxuwang 公开音频题页 <https://www.wuxuwang.com/yljg>；样本版权归 Amazon 所有，仅用于研究与教学。
