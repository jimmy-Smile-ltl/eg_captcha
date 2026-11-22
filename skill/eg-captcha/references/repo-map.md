# 仓库索引（快照 2026-11-21）

> 以下路径均相对仓库根目录 `EG_CAPTCHA_ROOT`。
> 数字与状态以仓库 `README.md` 和各案例 `meta.json` 为准；这里只做路由，不复制正文。

## 关键路径

| 内容 | 路径 |
|---|---|
| 总览、统计、快速导航 | `README.md` |
| 全量类型清单（厂商名链到类型页） | `docs/types.md` |
| 全部官方/第三方 demo | `docs/demos.md` |
| 整理规则、案例契约、文档规则 | `CONTRIBUTING.md` |
| 方法/成本/证据/失败档案 | `docs/00-methodology.md` |
| 厂商索引（类型表、代表样本、厂商结论） | `docs/<vendor>.md` |
| 类型页（题面、样本、思路、伪代码、结果、失败） | `cases/<vendor>/<type>/README.md` |
| 单个案例元数据 | `cases/<vendor>/<type>/<NNN>/meta.json` |
| 人工审查字段 | 各案例 `meta.json` 的 `review` |

## 厂商 → 文档

| slug | 厂商 | 厂商索引 | 类型数 | 样本 | 方法重点 |
|---|---|---|---:|---:|---|
| `geetest` | 极验 GeeTest | `docs/geetest.md` | 4 | 48 | CV（slide/icon）、PROTO（棋盘） |
| `yidun` | 网易易盾 | `docs/yidun.md` | 12 | 120 | CV、DET+OCR+VLM、DET+CV+VLM、DET+OCR+CV、VLM+CV |
| `tencent` | 腾讯云验证码 | `docs/tencent.md` | 5 | 48 | CV、DET+OCR+VLM；invisible 仅文档 |
| `shumei` | 数美 | `docs/shumei.md` | 5 | 56 | CV、DET+OCR+VLM、DET+OCR+CV（word-order 零 LLM） |
| `dingxiang` | 顶象 | `docs/dingxiang.md` | 13 | 156 | CV、CV+VLM、VLM；rotate/voice/word-order 无求解器 |
| `tianai` | 天爱 | `docs/tianai.md` | 14 | 156 | CV、CV+VLM、VLM+ACT、DET+OCR+VLM；pow 仅文档 |
| `cup` | 中国银联 | `docs/cup.md` | 5 | 48 | CV、VLM、CV+VLM；slide-no-image 仅文档 |
| `qingtongbu` | 企讯通 | `docs/qingtongbu.md` | 6 | 84 | CV、DET+OCR+CV、CV+ACT；rotate 无求解器 |
| `kgcaptcha` | 凯格 | `docs/kgcaptcha.md` | 5 | 60 | DET+CV+OCR（文字点选 weak）；其余 NONE |
| `meituan` | 美团 Yoda | `docs/meituan.md` | 5 | 60 | CV、DET+OCR（全部 partial：图像解完、提交未通） |
| `amazon` | Amazon AWS WAF | `docs/amazon.md` | 3 | 24 | VLM（grid）、ASR（audio）；token 仅文档 |
| `recaptcha` | Google reCAPTCHA | `docs/recaptcha.md` | 4 | 36 | VLM（网格）、ASR（音频）；token/v3 仅文档 |
| `hcaptcha` | hCaptcha | `docs/hcaptcha.md` | 3 | 36 | 全部 NONE，建议 VLM |
| `human-security` | HUMAN Security | `docs/human-security.md` | 3 | 8 | ACT（按压/无感）；邮件码 NONE |
| `datadome` | DataDome | `docs/datadome.md` | 2 | 13 | AUDIO+ASR（音频）；滑块 NONE |
| `mtcaptcha` | MTCaptcha | `docs/mtcaptcha.md` | 1 | 12 | ASR+OCR+VLM |
| `baidu` | 百度 | `docs/baidu.md` | 1 | 12 | CV+PROTO（旋转） |
| `bilibili` | 哔哩哔哩 B站 | `docs/bilibili.md` | 2 | 18 | CV（极验 v3 word / v4 icon-pixel） |

> “类型数”含 documented-only 类型；有案例的类型目录见第三节快照（86 个）。

## 无题面媒体 / documented-only

- 易盾 `invisible`、`sms`
- 腾讯 `invisible`
- 银联 `slide-no-image`
- 天爱 `pow`
- Amazon `token`
- reCAPTCHA v3 `token`

美团 `slider_71` 无图，合并在 `slide` 的说明中。

## 方法标签怎么读

- 厂商索引与类型页的“方法”列就是标签；`NONE(...)` 表示没有求解器，括号内是建议路线。
- 详细成本与选型见 `method-cost.md`；具体类型的题面、算法与结果，读 `cases/<vendor>/<type>/README.md`；厂商级汇总读 `docs/<vendor>.md`。
- 单案例的求解器/审查状态仍以 `meta.json` 为准。

## Demo 入口

官方/第三方可体验页面汇总在 `docs/demos.md`。需要给用户实际体验链接时，从那里取，不要凭记忆编 URL。

## 快照统计

- 18 家厂商、93 个厂商×类型组合（86 个有案例 + 7 个 documented-only）、995 个案例。
- 案例状态：641 `sample-only` + 318 `result-pending-review` + 36 `solved`。
- 人工审查：通过 703 / 部分正确 10 / 未通过 32 / 待审 146 / 不适用 104。
- 求解器（案例级）：`exists` 668 / `weak` 80 / `partial` 72 / `none` 168 / `browser` 7。
- 以 `README.md` 当前数字为准，更新仓库后请重读。
