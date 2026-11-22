# 数美 — 厂商索引

> 厂商：数美（Shumei）| 来源：<https://www.ishumei.com/new/product/tw/code> | 5 个类型（全部有案例）| 56 组案例
>
> 数美的公开演示样本，覆盖滑动拼图、文字点选、图标点选、语序点选和空间推理。`icon-click` 是本库 A 档纯 CV 代表（14/14、最大误差 ≤7px）；`word-order` 为零 LLM 词库约束求解（10/10 离线）；`slide` 受服务端轨迹 ML 限制，图像解无效。
>
> 返回 [README](../README.md) · [全量类型](types.md) · [方法论](00-methodology.md) · 详细内容在类型页（下方表内链接）。

## 一、类型总表

| 类型 | 中文 | 变体 | 样本 | 方法 | 求解器 / 源记录 | 审查 | 说明与详情 |
|---|---|---|---:|---|---|---|---|
| `slide` | 滑动拼图 | classic | 10 | CV+ACT | 有求解器（weak）· 低 | 10 通过 | 600×300 背景 + 90×300 拼图块；CV 可定位缺口，但服务端轨迹 ML 100% 拒绝模拟轨迹。[类型页](../cases/shumei/slide/README.md) |
| `text-click` | 文字点选 | classic | 12 | DET+OCR+VLM / DET+OCR | 弱求解（10 weak）· 低；011/012 exists · 高 | 12 通过 | 风格化空心字；前 10 组 OCR 0%、bbox 偏差大；011/012 本地 RapidOCR 4/4、零 LLM。[类型页](../cases/shumei/text-click/README.md) |
| `icon-click` | 图标点选 | classic | 14 | CV | 有求解器（exists）· 高 | 14 通过 | 600×300 背景 + 148×40 参考条；红掩膜 + 投影切分 + 10°×0.55–1.19 二值 IoU + 14px 聚类 + 匈牙利；源记录 14/14 ≤7px，本库 14 组 `offline-verified`。[类型页](../cases/shumei/icon-click/README.md) |
| `word-order` | 语序点选 | classic | 10 | DET+OCR+CV | 有求解器（exists）· 高 | 10 通过 | 600×300 照片 + 4 个彩色字；YOLO 定位（并框拆分/细长笔画）+ 颜色轮廓 + RapidOCR/ddddocr + 33,753 条词库（成语+四字常用语）4! 匹配；源记录 10/10 high，纠正旧 LLM 的 002 判定（实至名归→名存实亡）。[类型页](../cases/shumei/word-order/README.md) |
| `spatial` | 空间推理 | classic | 10 | CV | 有求解器（exists）· 高 | 10 通过 | 600×300 灰底 3D 几何体；颜色/形状规则 + 面积极值；源记录仅 2/2，不能外推成 10/10。[类型页](../cases/shumei/spatial/README.md) |

> 方法标签：`CV`=传统视觉；`DET`=目标检测；`OCR`=文字识别；`VLM`=多模态 LLM；`PROTO`=协议/枚举；`ACT`=行为/轨迹；`NONE`=无求解器（括号内为建议路线）。准确率一律为**源记录**，不代表本库已复核。

## 二、怎么读这张表

- **样本列**是案例目录数，五类共 56 组，每组至少含 `meta.json` + `bg.jpg`。
- **求解器列**按 `meta.solver.status` 汇总：`icon-click`、`spatial`、`word-order` 为 `exists`，`slide` 为 `weak`；`text-click` 是 10 组 `weak` + 2 组 `exists`，不能写成整类已解。
- **审查列**按 `meta.review` 现算：`slide` 10、`text-click` 12、`icon-click` 14、`word-order` 10、`spatial` 10 通过。
- **证据等级**：仅 `icon-click` 记录 `offline-verified` 且有 14 组 `answer.txt` / `result.json`；`word-order` 新增 10 组 `answer.txt` / `result.json` 但为 `unverified`；其余三类 `meta.evidence` 为空，只能引用源记录。
- **详情**在每行最后一列，点进去看题面、样本图、算法步骤、伪代码（仅 CV 路线）与失败记录。

## 三、案例目录

每个类型以 001 为例；除此之外每组至少含 `meta.json` 与 `bg.jpg`。

| 类型 | 001 目录内容 |
|---|---|
| `slide` | `bg.jpg` + `aux-01.png` + `solved.png` + `meta.json` |
| `text-click` | `bg.jpg` + `annotated.png` + `process-01-candidates.png` + `meta.json` |
| `icon-click` | `bg.jpg` + `aux-01.png` + `annotated.png` + `process-01-split.png` + `answer.txt` + `result.json` + `meta.json` |
| `word-order` | `bg.jpg` + `annotated.png` + `detect_stages.png` + `cv_annotated.png` + `cv_glyphs.png` + `ocr_votes.png` + `order_candidates.png` + `answer.txt` + `result.json` + `meta.json` |
| `spatial` | `bg.jpg` + `annotated.png` + `meta.json` |

## 四、代表样本

每类取 1 组，点击「详情」进入类型页；完整样本见 `cases/shumei/`。

| 类型 | 方法 | 代表样本 | 详情 |
|---|---|---|---|
| `slide` | CV+ACT | <img src="../cases/shumei/slide/001/bg.jpg" width="150" alt="数美滑动拼图 001"> | [详情](../cases/shumei/slide/README.md) |
| `text-click` | DET+OCR+VLM | <img src="../cases/shumei/text-click/001/bg.jpg" width="150" alt="数美文字点选 001"> | [详情](../cases/shumei/text-click/README.md) |
| `icon-click` | CV | <img src="../cases/shumei/icon-click/001/bg.jpg" width="150" alt="数美图标点选 001"> | [详情](../cases/shumei/icon-click/README.md) |
| `word-order` | DET+OCR+CV | <img src="../cases/shumei/word-order/001/bg.jpg" width="150" alt="数美语序点选 001"> | [详情](../cases/shumei/word-order/README.md) |
| `spatial` | CV | <img src="../cases/shumei/spatial/001/bg.jpg" width="150" alt="数美空间推理 001"> | [详情](../cases/shumei/spatial/README.md) |

## 五、厂商级结论

- **A 档主线是图标点选**：`icon-click` 用红掩膜、投影切分、角度/尺度搜索、二值 IoU 和匈牙利一对一，14/14 组最大误差 ≤7px，方法可复用、参数可解释。
- **文字点选是两条路线**：前 10 组空心字导致 OCR 0%，011/012 换本地 RapidOCR 后 4/4，但类型整体仍是 `weak`，不能报 12/12。
- **语序点选可零 LLM 闭环**：DET 定位 + 双 OCR 票数 + 33,753 条词库（成语 + 四字常用语）4! 匹配，10/10 离线 high；两个专项是并框拆分（002 名/存）与细长笔画（005 一）。旧 VLM 的 002「实至名归」被逐字 OCR 纠正为「名存实亡」。
- **滑动拼图卡在行为层**：CV 能找到缺口（001 `gap_x=329`、005 `gap_x=319`），服务端轨迹 ML 拒绝模拟轨迹，图像答案没有独立价值。
- **空间推理样本少、证据薄**：CV 规则只记录 2/2，10 组标注齐但无逐组结果；「最小」依赖投影面积，遮挡与透视是主要风险。
- **证据边界**：只有 `icon-click` 有 14 组 `answer.txt` / `result.json` 与 `evidence=offline-verified`；`word-order` 新增 10 组答案/结果但为 `unverified`；其余三类没有答案/结果文件，准确率只能标「源记录」。

## 六、来源与版权

来源：<https://www.ishumei.com/new/product/tw/code>。样本采集日期见各案例 `meta.json`；样本版权归数美所有，仅用于研究与教学展示。

> 统计截至 2026-11-21，以案例 `meta.json` 为准。
