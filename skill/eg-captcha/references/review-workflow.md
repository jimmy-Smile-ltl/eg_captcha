# 人工审查回填流程

## 字段

每个案例 `meta.json` 的相关字段：

| 字段 | 值 |
|---|---|
| `status` | `sample-only` / `result-pending-review` / `solved`（结果/媒体收录程度，审核不改它） |
| `solver.status` | `none` / `exists` / `weak` / `partial` / `browser` |
| `solver.accuracy` | `unknown` / `low` / `medium` / `high`（源记录，不代表已复核） |
| `solver.notes` | 未撰写原因、效果差原因、提交闭环状态 |
| `review.state` | `pending` / `not-applicable` → 审查后 `pass` / `partial` / `fail` |
| `review.result` | `correct` / `partial` / `incorrect`（viewer 评分码） |
| `review.reviewed_at` | 审查日期 `YYYY-MM-DD` |
| `review.notes` | 审查备注 |

## 状态机

- `sample-only`：只有样本（或只有可视化结果图）；`review.state` 初始 `pending` / `not-applicable`。
- `result-pending-review`：有求解器输出（`answer.txt` / `result.json`，可能有 `annotated.png`）；`review.state: pending`。
- 人工审核只改 `review.state` / `review.result` / `review.reviewed_at` / `review.notes`；
  **不改 `status`**——`status` 表示结果/媒体收录程度，不表示审核质量。
- 审查通过：`review.state: pass`、`review.result: correct`。
- 部分正确：`review.state: partial`、`review.result: partial`。
- 审查不通过：`review.state: fail`、`review.result: incorrect`；保留原结果作为失败证据，不要删除。

> viewer 在 localhost 使用 `scripts/serve.mjs` 时，点审核会自动写 `meta.json`（三层：localStorage → meta → 可选 Worker KV）。本流程用于批量回填、修正、或从审核导出 JSON 恢复。

## 回填步骤

1. 确认厂商、类型、案例编号、用户结论。用户说“全部通过”时，先确认范围（哪些类型/编号），不要擅自批量。
2. 读 `cases/<vendor>/<type>/<NNN>/meta.json`；有结果时同时读 `answer.txt` / `result.json`。
3. 更新：
   - `review.state`: `pass` / `partial` / `fail`
   - `review.result`: `correct` / `partial` / `incorrect`
   - `review.reviewed_at`: 审查日期 `YYYY-MM-DD`
   - `review.notes`: 证据/备注（如“人工听音频一致”“与标注图一致”）
   - **不要改 `status`**（结果/媒体收录程度与审核质量分开）
4. 校验：
   - `case_id` 与路径一致；
   - `review.state` / `review.result` 在枚举内；
   - 没有写入 token/cookie/密钥/个人信息；
   - 没有伪造答案或把 ASR/VLM 建议写成真值。
5. 报告改动的文件和字段；不要改 `solver.status` / `solver.accuracy`，除非用户明确给出新的求解器证据。

## 示例

用户说：`recaptcha/audio/001 人工听音频，转写一致，标为通过。`

审查通过后的字段：

| 字段 | 值 |
|---|---|
| `status` | `result-pending-review`（保持不变） |
| `review.state` | `pass` |
| `review.result` | `correct` |
| `review.reviewed_at` | `2026-11-07` |
| `review.notes` | `人工听 audio.mp3 与转写一致` |

用户说：`recaptcha/audio/001 转写错了，实际是 ...`

处理：`review.state: fail`、`review.result: incorrect`、`status` 保持不变，`review.notes` 写实际答案/结论与差异；保留原 `answer.txt` 作为失败证据。

## 不要做

- 不要在没有用户结论时自行标 `pass`/`fail`。
- 不要用 ASR/VLM 的置信度代替人工审查。
- 不要批量改多个案例，除非用户明确列出范围。
- 不要把 `sample-only` 案例补上猜测答案来“通过审查”。
