# 全量类型清单

> 按交互族分组；同一交互、同一答案空间只保留一个主 slug，变体写进案例 `meta.json`。
> 厂商覆盖为当前收录，不代表该厂商只有这些类型。
>
> 返回 [README](../README.md) · 方法/成本见 [00-methodology.md](00-methodology.md)

## 滑动 / 拼图

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `slide` | 滑动拼图 | 极验 / 腾讯 / 数美 / 顶象 / 天爱 / 银联 / 企讯通 / 凯格 / 美团 / DataDome |
| `slide-no-image` | 无图滑块 | 银联 |
| `slide-inference` | 滑动推理 | 美团 |
| `slide-restore` | 滑动还原 | 顶象 / 天爱 |
| `slide-rotate` | 旋转拼图 | 易盾 |
| `jigsaw` | 拼图（易盾为缺口滑块） | 易盾 / 顶象 / 天爱 |
| `inference` | 8 块换位乱序拼图 | 易盾 |
| `rotate` | 旋转验证 | 顶象 / 天爱 / 银联 / 企讯通 |
| `angle` | 角度验证 | 天爱 / 企讯通 |
| `curve` | 曲线滑动 | 天爱 / 企讯通 |
| `gesture` | 手势轨迹 | 天爱 |
| `scratch` | 刮刮卡 | 顶象 / 天爱 |

> 易盾：源目录 `jigsaw` 下 12 组全部是缺口滑块，归入 `jigsaw`；源目录 `inference` 是 8 块换位乱序拼图，独立归入 `inference`。

## 点选 / 识别

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `click` | 通用点选 | 银联 |
| `icon-click` | 图标点选 | 极验 / 易盾 / 腾讯 / 数美 / 顶象 / 天爱 / 企讯通 / 美团 |
| `text-click` | 文字点选 | 易盾 / 腾讯 / 数美 / 顶象 / 天爱 / 企讯通 / 凯格 / 银联 |
| `word-order` | 语序点选 | 易盾 / 数美 / 顶象 / 天爱 / 凯格 / 企讯通 |
| `word-group` | 语序选词 | 易盾 |
| `scene-click` | 场景语义点选 | 银联 |
| `diff-click` | 差异点击 | 顶象 |
| `area` | 面积验证 | 顶象 |
| `spatial` | 空间推理 | 易盾 / 腾讯 / 数美 / 顶象 / 凯格 |
| `font` | 字体识别 | 顶象 / 凯格 |
| `line-connect` | 最短线连点 | 美团 |
| `avoid` | 障碍躲避 | 易盾 |
| `text` | 字符识别 | 美团 / MTCaptcha |

## 网格 / 拖拽

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `grid` | 九宫格多选 | Amazon |
| `grid-independent` | 独立子图九宫格 | reCAPTCHA |
| `grid-split` | 整图切分九宫格 | reCAPTCHA |
| `binary` | 九宫格二分类 | hCaptcha |
| `drag-drop` | 拖拽拼块 | hCaptcha |
| `area-select` | 区域点选 | hCaptcha |

## 游戏 / 语音

| 类型 | 中文 | 厂商覆盖 |
|---|---|---|
| `gobang` | 五子棋 | 极验 |
| `icon-crush` | 图标消除 | 极验 |
| `voice` | 语音验证 | 顶象 |
| `audio` | 音频听写 | DataDome / Amazon / reCAPTCHA |

## 协议 / 无感

| 类型 | 中文 | 厂商覆盖 | 媒体 |
|---|---|---|---|
| `pow` | 工作量证明 | 天爱 | 无 |
| `token` | 指纹 / Token 挑战 | Amazon / reCAPTCHA（v3） | 无 |
| `invisible` | 无感 | 腾讯 / 易盾 / HUMAN | 无 |
| `press-hold` | 按压 | HUMAN | 截图 |
| `accessible-email` | 无障碍邮件码 | HUMAN | 无 |
| `sms` | 短信上行 | 易盾 | 无 |
| `random` | 智能随机 | 企讯通 / 天爱 | 模式 |
