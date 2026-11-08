---
name: eg-captcha
description: 查询与使用 eg_captcha 验证码知识库：按厂商/类型给出样本位置、方法标签（CV/DET/OCR/VLM/ASR/PROTO/ACT）、是否借助多模态 LLM、成本建议、求解器状态与人工审查回填。TRIGGER when 用户提到 eg_captcha、验证码知识库、验证码厂商/类型、方法选型、成本、样本审查、review 回填，或询问“这个验证码怎么解/用什么方法/有没有样本/准确率如何”。不用于直接实现求解器或写求解代码；需要代码时转 captcha-solver。
---

# eg_captcha 知识库

## 这是什么

`eg_captcha` 是一个**零代码验证码知识库**：只收样本、标注/结果、元数据与中文文档，不收求解器、脚本或密钥。它按“厂商 → 类型 → 案例”组织，每个类型标注：

- 方法标签：`CV` / `DET` / `OCR` / `VLM` / `ASR` / `PROTO` / `ACT` / `AUDIO` / `NONE`
- 是否借助多模态 LLM
- 求解器状态：`none` / `exists` / `weak` / `partial` / `browser`
- 人工审查状态：`pending` / `not-applicable`（等待回填）

当前快照（以仓库 README 为准）：16 家厂商、88 个厂商×类型组合、896 个案例（824 个 `sample-only` + 72 个 `result-pending-review`）。

## 定位仓库

按以下顺序确定 `EG_CAPTCHA_ROOT`：

1. 环境变量 `EG_CAPTCHA_ROOT`；
2. 若本文件位于仓库 `skill/eg-captcha/` 内，仓库根目录是本文件所在目录的 `../../`；
3. 若安装到全局 `~/.claude/skills/eg-captcha/` 且未设环境变量，读取同目录 `references/local-path.md`（若存在）；否则请用户提供仓库路径。

本 skill 只做索引和路由，不复制 `docs/` 正文。

## 读取顺序

不要一次加载全部文档；按问题逐层读：

1. 先读 `README.md`：当前统计、厂商总表、方法/成本图例、demo 入口。
2. 再读 `docs/<vendor>.md`：该厂商的类型表（方法、求解器状态、审查、思路说明）。
3. 需要具体样本时读 `cases/<vendor>/<type>/<NNN>/meta.json`。
   - `sample-only`：只有 `meta.json` + 媒体，没有答案/标注/结果。
   - `result-pending-review`：另有 `answer.txt` / `result.json`，可能有 `annotated.png`。
4. 需要方法论或失败档案时读 `docs/00-methodology.md`；需要整理规则时读 `CONTRIBUTING.md`。
5. 需要全量类型清单读 `docs/types.md`；需要官方/第三方体验入口读 `docs/demos.md`。

## 回答模板

### A. 方法选型 / 成本

按顺序回答，别只给“能解/不能解”：

1. 类型与答案空间：用户看到什么，服务端要什么输出。
2. 方法标签与理由：为什么选 CV / DET / OCR / VLM / ASR / PROTO。
3. 是否用 LLM：用了就明说；混合方案写清 VLM 只在哪一步调用。
4. 成本：VLM 起步便宜（无需训练、按调用计费）；量大时 CV 更便宜；混合常见。
5. 样本与文档：给出 `docs/<vendor>.md` 与代表性案例路径。
6. 状态与边界：`sample-only` / `result-pending-review` / `weak` / `partial` / `browser` 如实说。

### B. 样本 / 类型查询

先确认厂商与类型（用 `repo-map.md` 或 README 的类型清单），再给：

- 该类型的样本数、方法标签、求解器状态、审查状态；
- 代表案例路径；
- 如果该类型是 `NONE(...)`，说明未撰写原因，并给出建议路线（通常是先试 VLM）。

### C. 人工审查回填

只有用户明确给出审查结论时才改 `meta.json`，流程见 `references/review-workflow.md`：

- 确认厂商/类型/案例编号与结论；
- 更新 `review.state` / `review.result` / `review.reviewed_at` / `review.notes`；
- 通过 → `status: reviewed-pass`；不通过 → `reviewed-fail`；
- 校验枚举、路径、答案/结果一致性；不要伪造答案或把 ASR/VLM 建议当真值。

## 方法标签速查

| 标签 | 方法 | 用 LLM？ | 成本 |
|---|---|---|---|
| `CV` | 传统视觉：边缘/模板/颜色/几何 | 否 | 最低；量大更省 |
| `DET` | 目标检测 / 物体识别 | 否 | 中；需权重/标注 |
| `OCR` | 文字识别 / 字形匹配 | 否 | 低-中 |
| `VLM` | 多模态 LLM / 视觉推理 | **是** | 起步最便宜；按调用计费 |
| `ASR` | 语音识别 / 说话人分离 | 否（专用模型） | 中 |
| `PROTO` | 协议 / 枚举 / 计算 | 否 | 低 |
| `ACT` | 行为 / 轨迹 / 浏览器交互 | 否 | 低 |
| `NONE(...)` | 无求解器；括号内是建议路线 | — | — |

**成本判断**：VLM 不需要训练数据和标注，适合语义题、长尾题、网格题；量大时 CV 更便宜、更稳定。常见组合是 CV/OCR 先定位，VLM 只对分歧点或语义点调用（如 `DET+OCR+VLM`）。

## 边界

- **不写求解器代码**，不提供密钥、签名实现、参数构造或 token 回放；需要实际求解时转 `captcha-solver`。
- **不把 `sample-only` 说成已解**；没有答案就是没有答案。
- **不把 ASR/VLM 输出当真值**；只能说“建议/待审”，除非用户明确人工确认。
- **不批量改 review** 除非用户明确给出适用范围（例如“recaptcha 全部音频都通过”也要确认具体案例）。
- 回答时引用 `docs/<vendor>.md` 与案例路径，不要复制整篇文档或整库内容。

## 参考文件

- `references/repo-map.md` — 16 家厂商 → 文档/类型/样本/方法的索引。
- `references/method-cost.md` — 方法标签、选型规则、成本模型。
- `references/review-workflow.md` — `meta.json` 字段、审查状态机、校验清单。
