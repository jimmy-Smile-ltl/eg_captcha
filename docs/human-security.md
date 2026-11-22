# HUMAN Security — 厂商索引

> 厂商：HUMAN Security（PerimeterX）| 来源：<https://www.walmart.com/blocked> | 3 个类型（均有案例）| 8 组案例
>
> 风控/反自动化厂商，题型不是图像识别，而是页面级行为验证：`press-hold` 按住按钮、`invisible` 无感放行；`accessible-email` 为无障碍邮件码分支。本库的方法重点是 **ACT + 真实浏览器**，纯协议不可行。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细内容在类型页（下方表内链接）。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `press-hold` | 按压 | press-hold | 5 | ACT | 需浏览器（browser）· 高 | pending 5 | 真实浏览器按住按钮；纯 HTTP 被拒。[类型页](../cases/human-security/press-hold/README.md) |
| `invisible` | 无感 | invisible | 2 | ACT | 需浏览器（browser）· 高 | pending 2 | 零注入 Chrome 放行；无题面。[类型页](../cases/human-security/invisible/README.md) |
| `accessible-email` | 无障碍邮件码 | accessible-email | 1 | NONE(OCR/VLM) | 无求解器（none）· 未知 | not-applicable 1 | 邮件码分支；现有样本仅入口页。[类型页](../cases/human-security/accessible-email/README.md) |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**，不代表本库已复核；审查统计按案例 `meta.review` 现算（截至 2026-11-21）。

## 二、方法重点

- **浏览器是主路线**：`press-hold` 与 `invisible` 都不是可离线提交的题；源记录分别是「真实浏览器可解；纯 HTTP 被拒」与「零注入 Chrome 可过」。
- **没有图像题面**：`press-hold` 只有一个按钮，`invisible` 连按钮都没有；CV/OCR 没有目标，能做的是定位控件或直接让页面自然加载。
- **行为信号优先**：按压时长、指针事件、环境与 Cookie 都可能参与判定；源记录没有展开具体风控项。
- **邮件码分支未采集**：`accessible-email` 的唯一截图是「按住」入口页，不是邮件码界面，OCR/VLM 路线缺少输入。
- **证据只有截图**：8 组全部 `sample-only`，没有逐组结果；`accuracy=high` 只是源记录标签，没有数字。

## 三、类型页导读

| 类型 | 类型页重点 |
|---|---|
| `press-hold` | 静止态/按住态/通过后页面、按住时长未知、真实浏览器路线 |
| `invisible` | 页面级放行、无题面、两组页面状态、零注入 Chrome |
| `accessible-email` | 入口页与邮件码的样本缺口、补采优先、OCR/VLM 路线 |

## 四、代表样本

每类取 1 组，点击「详情」进入类型页；完整样本见 `cases/human-security/`。

| 类型 | 方法 | 代表样本 | 详情 |
|---|---|---|---|
| `press-hold` | ACT | <img src="../cases/human-security/press-hold/001/bg.png" width="220" alt="HUMAN Security 按压 001 入口"> | [详情](../cases/human-security/press-hold/README.md) |
| `invisible` | ACT | <img src="../cases/human-security/invisible/001/bg.png" width="220" alt="HUMAN Security 无感 001"> | [详情](../cases/human-security/invisible/README.md) |
| `accessible-email` | NONE(OCR/VLM) | <img src="../cases/human-security/accessible-email/001/bg.png" width="220" alt="HUMAN Security 无障碍邮件码 001（入口页）"> | [详情](../cases/human-security/accessible-email/README.md) |

> `accessible-email` 的缩略图是「按住」入口页，与 `press-hold/003/bg.png` 逐字节一致；它不是邮件码界面，样本缺口见类型页。

## 五、厂商级结论

- **不是纯协议厂商**：本库没有可计算的答案串或签名参数；`press-hold`/`invisible` 的产出一个是交互动作，一个是浏览器会话。
- **审查待回填**：`press-hold` 5 组、`invisible` 2 组均 `pending`；`accessible-email` 为 `not-applicable`。
- **数据缺口**：`accessible-email` 复用了 `press-hold` 入口页，没有任何邮件码图像；该类型目前只能记录「形态确认、未复现」。
- **证据边界**：截图能证明页面形态，不能证明通过率；不要把 `accuracy=high` 当成已复核的数值。
- **后续优先**：先补真实浏览器交互记录（含时间线/Cookie）与邮件码界面，再谈效果。

## 六、审查与证据

| 类型 | 样本 | 求解器 | 审查 | 证据等级 |
|---|---:|---|---|---|
| `press-hold` | 5 | browser · 高 | pending 5 | sample-only |
| `invisible` | 2 | browser · 高 | pending 2 | sample-only |
| `accessible-email` | 1 | none · 未知 | not-applicable 1 | sample-only |

> 8 组都没有逐组结果；`pending` 表示尚未人工复核，不是失败。`accuracy=high` 只有标签，没有数字。

## 七、使用建议

- 先读类型页第 3 节的截图对照：哪张是静止态、哪张是按住中、哪张是通过后页面。
- 需要复现时按第 5 节的浏览器路线操作；不要尝试用纯 HTTP 提交，源记录已明确被拒。
- `accessible-email` 先补采邮件码界面，再谈 OCR/VLM；现有截图不是该类型的题面。
- 引用数据时以 `meta.json` 为准；本索引的统计截至 2026-11-21。

## 八、来源

来源：<https://www.walmart.com/blocked>。样本采集日期见各案例 `meta.json`；样本版权归 HUMAN Security / Walmart 所有，仅用于研究与教学展示。
