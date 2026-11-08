# 人工审查回填流程

## 字段

每个案例 `meta.json` 的相关字段：

| 字段 | 值 |
|---|---|
| `status` | `sample-only` / `result-pending-review` / `reviewed-pass` / `reviewed-fail` |
| `solver.status` | `none` / `exists` / `weak` / `partial` / `browser` |
| `solver.accuracy` | `unknown` / `low` / `medium` / `high`（源记录，不代表已复核） |
| `solver.notes` | 未撰写原因、效果差原因、提交闭环状态 |
| `review.state` | `pending` / `not-applicable` → 审查后 `pass` / `fail` |
| `review.result` | 审查结论 |
| `review.reviewed_at` | 审查日期 `YYYY-MM-DD` |
| `review.notes` | 审查备注 |

## 状态机

- `sample-only`：只有样本，没有答案/标注/结果；`review.state` 通常 `not-applicable`。
- `result-pending-review`：有求解器输出（`answer.txt` / `result.json`，可能有 `annotated.png`）；`review.state: pending`。
- 审查通过：`review.state: pass`，`status: reviewed-pass`。
- 审查不通过：`review.state: fail`，`status: reviewed-fail`；保留原结果作为失败证据，不要删除。
- `sample-only` 不允许直接改成 `reviewed-pass`；必须先有答案/结果。

## 回填步骤

1. 确认厂商、类型、案例编号、用户结论。用户说“全部通过”时，先确认范围（哪些类型/编号），不要擅自批量。
2. 读 `cases/<vendor>/<type>/<NNN>/meta.json`；有结果时同时读 `answer.txt` / `result.json`。
3. 更新：
   - `review.state`: `pass` / `fail`
   - `review.result`: 用用户给的原话或简短结论
   - `review.reviewed_at`: 当天日期
   - `review.notes`: 证据/备注（如“人工听音频一致”“与标注图一致”）
   - `status`: `reviewed-pass` / `reviewed-fail`
4. 校验：
   - `case_id` 与路径一致；
   - `review.state` / `status` 在枚举内；
   - `reviewed-pass` 应有 `answer.txt`；
   - 没有写入 token/cookie/密钥/个人信息；
   - 没有伪造答案或把 ASR/VLM 建议写成真值。
5. 报告改动的文件和字段；不要改 `solver.status` / `solver.accuracy`，除非用户明确给出新的求解器证据。

## 示例

用户说：`recaptcha/audio/001 人工听音频，转写一致，标为通过。`

审查通过后的字段：

| 字段 | 值 |
|---|---|
| `status` | `reviewed-pass` |
| `review.state` | `pass` |
| `review.result` | `人工听音频一致` |
| `review.reviewed_at` | `2026-11-07` |
| `review.notes` | `人工听 audio.mp3 与转写一致` |

用户说：`recaptcha/audio/001 转写错了，实际是 ...`

处理：`review.state: fail`、`status: reviewed-fail`、`review.result` 写实际答案/结论，保留原 `answer.txt` 作为失败证据；在 `review.notes` 说明差异。

## 不要做

- 不要在没有用户结论时自行标 `pass`/`fail`。
- 不要用 ASR/VLM 的置信度代替人工审查。
- 不要批量改多个案例，除非用户明确列出范围。
- 不要把 `sample-only` 案例补上猜测答案来“通过审查”。
