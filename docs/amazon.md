# Amazon AWS WAF — 厂商索引

> 厂商：Amazon AWS WAF | 来源：<https://www.amazon.com/>（音频题另见 <https://www.wuxuwang.com/yljg>）| 3 个类型（2 个有案例、1 个仅文档）| 24 组案例
>
> 电商与 AWS WAF 场景下的图片/音频挑战：九宫格多选用视觉 LLM 直接读格，音频听写用双人分离 + 多模型 ASR 融合；token 只记录协议形态。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细拆解在类型页（下方表内链接）。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `grid` | 九宫格多选 | grid-5 | 12 | VLM | 有求解器（exists）· 高 | 待审 12 | 3×3 独立照片，按中文短题选格；VLM 输出格号；源记录 30/30 离线、20/20 线上。[类型页](../cases/amazon/grid/README.md) |
| `audio` | 音频听写 | noisyneighbour-en-us | 12 | ASR | 有求解器（exists）· 高 | 11 通过 / 1 待审 | 女声出题、男声旁白；SepFormer 分离 + F0 + 三模型 ASR；源记录 45/46 轮（61 提交 45 成功）。[类型页](../cases/amazon/audio/README.md) |
| `token` | 指纹 / Token 挑战 | token | 0 | PROTO | 有求解器（exists）· 高 | pending | 仅文档 / 无题面；PoW/token 协议，纯 HTTP token 链，无视觉样本。 |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**，不代表本库已复核；审查统计按案例 `meta.review` 现算（截至 2026-11-21）。

### 类型页速查

| 类型 | 先看什么 | 适合谁 |
|---|---|---|
| `grid` | 3×3 题面与源标注图 | 想快速验证 VLM 读格路线的人 |
| `audio` | 12 条音频 + 双人 F0 与词表 | 想做语音分离、多模型融合的人 |
| `token` | 无题面，仅协议形态 | 只关心 token 链的人 |

## 二、代表样本

每类取 1 组；音频没有题面图，用音频直链代替缩略图。完整样本见 `cases/amazon/`。

| 类型 | 方法 | 代表样本 | 详情 |
|---|---|---|---|
| `grid` | VLM | <img src="../cases/amazon/grid/001/bg.jpg" width="150" alt="Amazon 九宫格多选 001 题面"> | [详情](../cases/amazon/grid/README.md) |
| `audio` | ASR | [听音频 001](../cases/amazon/audio/001/audio.aac)（仅 001 有页面截图） | [详情](../cases/amazon/audio/README.md) |
| `token` | PROTO | —（仅文档，无题面） | 无案例目录 |

## 三、方法选择与审查

| 场景 | 建议路线 | 成本特征 | 本库状态 |
|---|---|---|---|
| 九宫格选物（`grid`） | 3×3 等分后让 VLM 直接输出格号，必要时逐格复问 | 一次调用可读全图，起步便宜 | 12 组仅样本；`solved.png` 为源标注，不是逐题真值 |
| 双人混音听写（`audio`） | SepFormer 分离 → F0 定角色 → 女声轨/原混音三模型转写 → 一致度定档 | 分离与多模型推理成本较高 | 12 组两个词已入库；11 通过 / 1 待审 |
| Token 挑战（`token`） | 纯 HTTP token 链，无视觉/音频模型 | 无题面，依赖协议细节 | 仅文档，样本 0 |

- **分类说明**：`grid` 的 9 格是各自独立的照片；`audio` 的两个词由女声念出，男声只负责背景旁白，不是「二选一说话人」；`token` 没有可收集的题面，因此不建案例目录。
- **审查口径**：`grid` 的 12 组 `review.state` 全为 `pending`；`audio` 有 11 组 `pass/correct`，002 仍 `pending`。两类的源记录准确率都只写在 `solver.notes` 中，本库没有逐题重跑。

## 四、厂商级结论

- **强项**：`grid` 的任务结构简单，视觉 LLM 一次读 9 格即可，不需要训练检测器；源记录在两批数据上均为满分（30/30 离线、20/20 线上）。
- **特色**：`audio` 不是普通听写，而是「女声出题 + 男声朗读文章」的双人混音。关键是先用 SepFormer 分出女声，再用 F0 确认角色（女声约 157–250 Hz、男声约 101–114 Hz），最后用三模型 ASR + 原混音复核定两个词。
- **已暴露的弱点**：音频第二词容易被分离失真带偏（003 的 `I mean it`、008 的 `connect/smart/right`、011 的 `please`），必须回到混音复核；12 组中 002 仍待人工听音。`grid` 本库只收样本，没有复制逐题结果，`solved.png` 只是源标注截图，不能当 ground truth。
- **证据边界**：`audio` 的 `evidence` 为 `unverified`，45/46 轮与 61 次提交 45 成功均为源记录；`grid` 的准确率同样是源记录，本库没有逐题答案，不能重算。

## 五、统计口径

- **样本数**：按 `cases/amazon/<type>/` 下的案例目录现算，`grid` 12、`audio` 12、`token` 0；共 24 组。
- **审查数**：`meta.review` 现算；`audio` 11 通过 / 1 待审，`grid` 12 待审，`token` 无案例。
- **源记录**：`solver.notes` 抄录，未在本库复跑；`grid` 30/30 离线、20/20 线上；`audio` 45/46 轮（61 次提交 45 成功）。
- **本库证据**：`audio` 为 `unverified`（离线 ASR 结果，11 通过 / 1 待审）；`grid` 为 `sample-only`（只收题面与源标注，不复制结果）。

## 六、来源

来源：Amazon AWS WAF 公开挑战页（amazon.com 等）与 wuxuwang 公开音频题页 <https://www.wuxuwang.com/yljg>。`grid` 001 采集于 2026-10-31、002–012 采集于 2026-11-01；`audio` 采集于 2026-11-07。样本版权归 Amazon 所有，仅用于研究与教学展示。
