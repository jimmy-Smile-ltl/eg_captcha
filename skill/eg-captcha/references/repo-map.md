# 仓库索引（快照 2026-11-07）

> 以下路径均相对仓库根目录 `EG_CAPTCHA_ROOT`。
> 数字与状态以仓库 `README.md` 和各 `docs/<vendor>.md` 为准；这里只做路由，不复制正文。

## 关键路径

| 内容 | 路径 |
|---|---|
| 总览、统计、快速导航 | `README.md` |
| 全量类型清单 | `docs/types.md` |
| 全部官方/第三方 demo | `docs/demos.md` |
| 整理规则、案例契约 | `CONTRIBUTING.md` |
| 方法/成本/证据/失败档案 | `docs/00-methodology.md` |
| 厂商类型表与方法 | `docs/<vendor>.md` |
| 单个案例元数据 | `cases/<vendor>/<type>/<NNN>/meta.json` |
| 人工审查字段 | 各案例 `meta.json` 的 `review` |

## 厂商 → 文档

| slug | 厂商 | 文档 | 类型数 | 样本 | 方法重点 |
|---|---|---|---:|---:|---|
| `geetest` | 极验 GeeTest | `docs/geetest.md` | 4 | 48 | CV（slide/icon）、PROTO（棋盘） |
| `yidun` | 网易易盾 | `docs/yidun.md` | 10 | 96 | CV、VLM+CV、DET+OCR+VLM、VLM+ACT |
| `tencent` | 腾讯云验证码 | `docs/tencent.md` | 5 | 48 | CV、DET+OCR+VLM |
| `shumei` | 数美 | `docs/shumei.md` | 5 | 50 | CV、DET+OCR+VLM（效果差） |
| `dingxiang` | 顶象 | `docs/dingxiang.md` | 13 | 156 | CV、VLM、CV+VLM |
| `tianai` | 天爱 | `docs/tianai.md` | 13 | 132 | CV、CV+VLM、VLM+ACT、DET+OCR+VLM |
| `cup` | 中国银联 | `docs/cup.md` | 5 | 48 | CV、VLM、PROTO/ACT |
| `qingtongbu` | 企讯通 | `docs/qingtongbu.md` | 7 | 84 | CV（angle）；其余 NONE |
| `kgcaptcha` | 凯格 | `docs/kgcaptcha.md` | 5 | 45 | 全部 NONE，建议 CV/OCR+VLM |
| `meituan` | 美团 Yoda | `docs/meituan.md` | 5 | 60 | CV、DET+OCR（部分攻克） |
| `amazon` | Amazon AWS WAF | `docs/amazon.md` | 3 | 24 | VLM（grid）、ASR（audio）、PROTO（token） |
| `recaptcha` | Google reCAPTCHA | `docs/recaptcha.md` | 4 | 36 | VLM（网格）、ASR（音频）、PROTO（v3） |
| `hcaptcha` | hCaptcha | `docs/hcaptcha.md` | 3 | 36 | 全部 NONE，建议 VLM |
| `human-security` | HUMAN Security | `docs/human-security.md` | 3 | 8 | ACT（按压/无感）；邮件码 NONE |
| `datadome` | DataDome | `docs/datadome.md` | 2 | 13 | AUDIO+ASR（音频）；滑块 NONE |
| `mtcaptcha` | MTCaptcha | `docs/mtcaptcha.md` | 1 | 12 | ASR+OCR+VLM |

## 无题面媒体 / documented-only

- 易盾 `invisible`、`sms`
- 腾讯 `invisible`
- 银联 `slide-no-image`
- 天爱 `pow`、`random`
- Amazon `token`
- reCAPTCHA v3 `token`

美团 `slider_71` 无图，合并在 `slide` 的说明中。

## 方法标签怎么读

- 文档类型表的“方法”列就是标签；`NONE(...)` 表示没有求解器，括号内是建议路线。
- 详细成本与选型见 `method-cost.md`。
- 具体类型的求解器状态、准确率（源记录）、审查状态，读对应 `docs/<vendor>.md` 的类型表。

## Demo 入口

官方/第三方可体验页面汇总在 `README.md` 的“验证码体验入口”。需要给用户实际体验链接时，从那里取，不要凭记忆编 URL。

## 快照统计

- 16 家厂商、88 个厂商×类型组合、896 个案例（824 `sample-only` + 72 `result-pending-review`）。
- 类型级求解器状态：`exists` 43 / `none` 29 / `weak` 7 / `partial` 7 / `browser` 2。
- 以 `README.md` 当前数字为准，更新仓库后请重读。
