# 百度 — 厂商索引

> 厂商：百度（站点 `baidu.com` / 爱企查）| 来源：<https://aiqicha.baidu.com/>（触发式 wappass spin）| 1 个类型（有案例）| 12 组案例
>
> 旋转验证码：拖动滑块把圆图转正。本库用 ConvNeXt Tiny 回归 `sin/cos` 预测当前角度，再走 `wappass` 的 PoW + 轨迹 + 双层 AES 纯协议提交；200 张标注 MAE 10.50°、acc@10° 81.0%（源记录）。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细内容在类型页（下方表内链接）。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `rotate` | 旋转验证 | spin | 12 | CV+PROTO | 有求解器（exists）· 高 | pending 12 | ConvNeXt Tiny（sin/cos）预测角度 + wappass 协议；200 张 MAE 10.50°、acc@10° 81.0%；12 组误差 ≤ 4.90°。[类型页](../cases/baidu/rotate/README.md) |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`ASR`=语音识别；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`AUDIO`=音频信号；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**，不代表本库已复核；审查统计按案例 `meta.review` 现算（截至 2026-11-21）。

## 二、方法重点

- **两条线**：图像线预测旋转角，协议线完成 `init → style → 取图 → 角度识别 → PoW → 轨迹 → 双层 AES → log`；只做角度识别不足以通过。
- **角度约定**：预测值 = 图片**当前被旋转的角度**；回正 = `rotate(+angle)`（PIL 逆时针）；协议提交比例为 `angle / 360`。
- **模型**：ConvNeXt Tiny `conv_0203_ft_0205_pl_0206`，`sin/cos` 回归头；200 张标注图微调 + 伪标签半监督。
- **指标**：200 张标注 MAE 10.50°、acc@10° 81.0%、acc@15° 84.5%；ResNet50 基线 MAE 82.97° 已弃用。
- **证据边界**：12 组是覆盖 60°–301° 的选样，不是随机评测；`solved.png` 是模型预测回正，不是真值回正。

## 三、类型页导读

| 类型 | 类型页重点 |
|---|---|
| `rotate` | 12 组逐条真值/预测/误差、sin/cos 公式、wappass 协议链、失败模式与证据边界 |

## 四、代表样本

每类取 1 组，点击「详情」进入类型页；完整样本见 `cases/baidu/`。

| 类型 | 方法 | 原图 / 模型回正 | 详情 |
|---|---|---|---|
| `rotate` | CV+PROTO | <img src="../cases/baidu/rotate/001/bg.png" width="130" alt="百度旋转验证 001 原图"> <img src="../cases/baidu/rotate/001/solved.png" width="130" alt="百度旋转验证 001 模型回正"> | [详情](../cases/baidu/rotate/README.md) |

> 12 组从 200 张人工标注真机图中选取，角度覆盖 60°–301°；`solved.png` 是模型按预测角回正，不是真值回正。

## 五、厂商级结论

- **图像线可离线评价**：152×152 圆图、连续角度回归；MAE 与 acc@10° 是主要指标，12 组选样误差均 ≤ 4.90°（平均 2.96°）。
- **协议线是门槛**：PoW、轨迹、双层 AES 与新鲜会话缺一不可；角度对了但轨迹/PoW 不匹配仍会失败。
- **上游与对照**：上游项目报告 1500 次真实通过率 73.33%；ResNet50 基线 MAE 82.97° 已弃用；本库 200 张标注的离线指标更保守。
- **没有本库线上证据**：12 组 `review=pending`、`evidence=manual`；历史 `op=1` 来自源目录，不在本库，不能代表任意时刻可复现。
- **难点**：连续角度误差、会话时效、Cookie/UA 一致性；模型在 200 张中 acc@10° 约 81%，个别样本仍会偏差较大。

## 六、审查与证据

| 类型 | 样本 | 求解器 | 审查 | 证据等级 |
|---|---:|---|---|---|
| `rotate` | 12 | exists · 高 | pending 12 | manual（人工标注真值 + 离线预测） |

> 12 组 `answer.txt` / `result.json` / `solved.png` 齐全，但审查尚未回填；`result.status=solved` 只表示离线推理完成，不等于线上通过。源记录 73.33% 通过率来自上游项目，本库没有提交日志。

## 七、使用建议

- 先读类型页第 2 节的角度约定：真值 = 当前旋转角，回正 = `rotate(+angle)`，提交比例 = `angle / 360`；约定错了后面全错。
- 图像线可离线复算；协议线（PoW/轨迹/双层 AES）源记录未展开，不要假设只靠模型就能过。
- 12 组是按角度覆盖选出的样例，引用时不要把它当成随机评测；效果以 200 张标注的 MAE 10.50°、acc@10° 81.0% 为准，并标「源记录」。
- 引用数据时以 `meta.json` 为准；本索引的统计截至 2026-11-21。

## 八、来源

- 样本来源：`solve_CAPTCHA/samples/other/baidu/rotate/sets/labeled`（200 张标注真机图）
- 选择与回正：12 组角度覆盖 60°–301°，`solved.png` 由 ConvNeXt 预测角度回正
- 指标：`samples/other/baidu/rotate/sets/demo/predictions_convnext.json`（200 张评测）
- 协议与模型说明：`samples/other/baidu/rotate/README.md`
- 版权归百度所有，仅用于研究与教学展示。
