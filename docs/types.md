# 全量类型清单

> 按交互族分组；同一交互、同一答案空间通常只保留一个主 slug，变体写进案例 `meta.json`。若厂商 API 用不同 `type` 码下发、且题面/参数机制不同（如天爱 `CURVE`/`CURVE2`/`CURVE3`），则拆成独立类型。
> 厂商名链到该厂商该类型的**类型页**（题面、样本、思路、结果）；标「仅文档」的类型没有案例目录，见厂商索引。
> 厂商覆盖为当前收录，不代表该厂商只有这些类型。
>
> 返回 [README](../README.md) · 方法/成本见 [00-methodology.md](00-methodology.md)

## 滑动 / 拼图

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `slide` | 滑动拼图 | [极验](../cases/geetest/slide/README.md) / [腾讯](../cases/tencent/slide/README.md) / [数美](../cases/shumei/slide/README.md) / [顶象](../cases/dingxiang/slide/README.md) / [天爱](../cases/tianai/slide/README.md) / [银联](../cases/cup/slide/README.md) / [企讯通](../cases/qingtongbu/slide/README.md) / [凯格](../cases/kgcaptcha/slide/README.md) / [美团](../cases/meituan/slide/README.md) / [DataDome](../cases/datadome/slide/README.md) |
| `slide-no-image` | 无图滑块 | 银联（仅文档） |
| `slide-inference` | 滑动推理 | [美团](../cases/meituan/slide-inference/README.md) |
| `slide-restore` | 滑动还原 | [顶象](../cases/dingxiang/slide-restore/README.md) / [天爱](../cases/tianai/slide-restore/README.md) |
| `slide-rotate` | 旋转拼图 | [易盾](../cases/yidun/slide-rotate/README.md) |
| `jigsaw` | 拼图（易盾为缺口滑块） | [易盾](../cases/yidun/jigsaw/README.md) / [顶象](../cases/dingxiang/jigsaw/README.md) / [天爱](../cases/tianai/jigsaw/README.md) |
| `inference` | 8 块换位乱序拼图 | [易盾](../cases/yidun/inference/README.md) |
| `rotate` | 旋转验证 | [百度](../cases/baidu/rotate/README.md) / [顶象](../cases/dingxiang/rotate/README.md) / [天爱](../cases/tianai/rotate/README.md) / [银联](../cases/cup/rotate/README.md) / [企讯通](../cases/qingtongbu/rotate/README.md) |
| `angle` | 角度验证 | [天爱](../cases/tianai/angle/README.md) / [企讯通](../cases/qingtongbu/angle/README.md) |
| `curve` | 曲线滑动 | [天爱](../cases/tianai/curve/README.md) / [企讯通](../cases/qingtongbu/curve/README.md) |
| `curve-ellipse` | 椭圆曲线 | [天爱](../cases/tianai/curve-ellipse/README.md) |
| `curve-bezier` | 双贝塞尔曲线 | [天爱](../cases/tianai/curve-bezier/README.md) |
| `gesture` | 手势轨迹 | [天爱](../cases/tianai/gesture/README.md) |
| `scratch` | 刮刮卡 | [顶象](../cases/dingxiang/scratch/README.md) / [天爱](../cases/tianai/scratch/README.md) |

> 易盾：源目录 `jigsaw` 下 12 组全部是缺口滑块，归入 `jigsaw`；源目录 `inference` 是 8 块换位乱序拼图，独立归入 `inference`。
>
> 易盾文字点选按源目录拆成两类：`text_click` → `text-click`（标准版，5 个独立字）；`text_click_new` → `text-click-enhanced`（增强版，约 6–7 字含重叠双字，12 组均给出按题面字序的 3 点结果）。增强版的布局不固定：多数组是 2 组重叠对 + 1 个独立字，006 经审核确认 3 个目标全在重叠对里；要点的永远是被压住的下层。对框有尺度上限：真对框是字径的 0.79–1.54 倍，超过 1.7 倍的框是背景块（011 的 88×93 框把整头小牛框了进来）。
>
> 天爱曲线按 API type 拆成三类：`CURVE` → `curve`（端点固定、可见参考曲线贴合；目标控制点 x 不在 p 里）、`CURVE2` → `curve-ellipse`（椭圆缩放）、`CURVE3` → `curve-bezier`（双贝塞尔）。官网入口只列一个「曲线」，但三者题面与参数机制不同。

## 点选 / 识别

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `click` | 通用点选 | [银联](../cases/cup/click/README.md) |
| `icon-click` | 图标点选 | [极验](../cases/geetest/icon-click/README.md) / [易盾](../cases/yidun/icon-click/README.md) / [腾讯](../cases/tencent/icon-click/README.md) / [数美](../cases/shumei/icon-click/README.md) / [顶象](../cases/dingxiang/icon-click/README.md) / [天爱](../cases/tianai/icon-click/README.md) / [企讯通](../cases/qingtongbu/icon-click/README.md) / [美团](../cases/meituan/icon-click/README.md) / [B站](../cases/bilibili/icon-click/README.md) |
| `text-click` | 文字点选 | [易盾](../cases/yidun/text-click/README.md) / [腾讯](../cases/tencent/text-click/README.md) / [数美](../cases/shumei/text-click/README.md) / [顶象](../cases/dingxiang/text-click/README.md) / [天爱](../cases/tianai/text-click/README.md) / [企讯通](../cases/qingtongbu/text-click/README.md) / [凯格](../cases/kgcaptcha/text-click/README.md) / [B站](../cases/bilibili/text-click/README.md) |
| `text-click-enhanced` | 增强文字点选 | [易盾](../cases/yidun/text-click-enhanced/README.md) |
| `word-order` | 语序点选 | [易盾](../cases/yidun/word-order/README.md) / [数美](../cases/shumei/word-order/README.md) / [顶象](../cases/dingxiang/word-order/README.md) / [天爱](../cases/tianai/word-order/README.md) / [凯格](../cases/kgcaptcha/word-order/README.md) |
| `word-group` | 语序选词 | [易盾](../cases/yidun/word-group/README.md) |
| `scene-click` | 场景语义点选 | [银联](../cases/cup/scene-click/README.md) |
| `diff-click` | 差异点击 | [顶象](../cases/dingxiang/diff-click/README.md) |
| `area` | 面积验证 | [顶象](../cases/dingxiang/area/README.md) |
| `spatial` | 空间推理 | [易盾](../cases/yidun/spatial/README.md) / [腾讯](../cases/tencent/spatial/README.md) / [数美](../cases/shumei/spatial/README.md) / [顶象](../cases/dingxiang/spatial/README.md) / [凯格](../cases/kgcaptcha/spatial/README.md) |
| `font` | 字体识别 | [顶象](../cases/dingxiang/font/README.md) / [凯格](../cases/kgcaptcha/font/README.md) |
| `line-connect` | 最短线连点 | [美团](../cases/meituan/line-connect/README.md) |
| `avoid` | 障碍躲避 | [易盾](../cases/yidun/avoid/README.md) |
| `text` | 字符识别 | [美团](../cases/meituan/text/README.md) / [MTCaptcha](../cases/mtcaptcha/text/README.md) |

## 网格 / 拖拽

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `grid` | 九宫格多选 | [Amazon](../cases/amazon/grid/README.md) |
| `grid-independent` | 独立子图九宫格 | [reCAPTCHA](../cases/recaptcha/grid-independent/README.md) |
| `grid-split` | 整图切分九宫格 | [reCAPTCHA](../cases/recaptcha/grid-split/README.md) |
| `binary` | 九宫格二分类 | [hCaptcha](../cases/hcaptcha/binary/README.md) |
| `drag-drop` | 拖拽拼块 | [hCaptcha](../cases/hcaptcha/drag-drop/README.md) |
| `area-select` | 区域点选 | [hCaptcha](../cases/hcaptcha/area-select/README.md) |

## 游戏 / 语音

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `gobang` | 五子棋 | [极验](../cases/geetest/gobang/README.md) |
| `icon-crush` | 图标消除 | [极验](../cases/geetest/icon-crush/README.md) |
| `voice` | 语音验证 | [顶象](../cases/dingxiang/voice/README.md) |
| `audio` | 音频听写 | [DataDome](../cases/datadome/audio/README.md) / [Amazon](../cases/amazon/audio/README.md) / [reCAPTCHA](../cases/recaptcha/audio/README.md) |

## 协议 / 无感

| 类型 | 中文 | 厂商覆盖 | 媒体 |
|---|---|---|---|
| `pow` | 工作量证明 | 天爱（仅文档） | 无 |
| `token` | 指纹 / Token 挑战 | Amazon（仅文档） / reCAPTCHA（v3，仅文档） | 无 |
| `invisible` | 无感 | 腾讯（仅文档） / 易盾（仅文档） / [HUMAN](../cases/human-security/invisible/README.md) | 无 |
| `press-hold` | 按压 | [HUMAN](../cases/human-security/press-hold/README.md) | 截图 |
| `accessible-email` | 无障碍邮件码 | [HUMAN](../cases/human-security/accessible-email/README.md) | 无 |
| `sms` | 短信上行 | 易盾（仅文档） | 无 |

> **`random`（智能随机）不是类型**：企讯通、天爱体验页的「智能随机」入口每次从该厂商其他类型中随机返回一种，无独立题面；只作为入口模式记录，不建类型、不建案例。
