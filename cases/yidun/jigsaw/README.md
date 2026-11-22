# 网易易盾 · 滑块拼图（jigsaw）

> 320×160 背景 + 60×158 拼图块，拖动滑块把拼图块补进背景缺口；masked Sobel 梯度 + alpha 掩膜 NCC 定位缺口，纯 CV、零 LLM。
>
> 返回：[易盾索引](../../../docs/yidun.md) · [全量类型](../../../docs/types.md) · [方法论](../../../docs/00-methodology.md)

## 一、速览

| 项 | 值 |
|---|---|
| 厂商 | 网易易盾（`yidun`） |
| 类型 / 变体 | `jigsaw` / classic |
| 中文 | 滑块拼图（缺口滑块） |
| 媒体 | 图片 |
| 样本 | 12 组（001–012） |
| 答案格式 | `offset-x`：缺口左上角 `(gap_x, gap_y)` |
| 方法 | `CV`（masked Sobel 梯度 + alpha mask NCC，无 LLM） |
| 求解器 | `exists` · 高（源记录 12/12 离线） |
| 人工审查 | 12 通过 / 0 错误（截至 2026-11-21） |
| 证据等级 | `offline-verified`（12 组 `result.json` 均为 high） |

## 二、题面与判定

用户拖动滑块，把左侧的拼图块补进背景中的缺口。缺口是 158px 高的长条，`gap_y` 只有 1–2px 的差异，实际答案几乎完全由 `gap_x` 决定。

| 案例 | 题面（`meta.prompt`） | 通过条件 | 答案 |
|---|---|---|---|
| [001](001/) | 拖动滑块，将拼图块对准背景缺口 | 该批案例未记录 `pass_criteria`；按缺口对齐判定 | `gap_x=89, gap_y=2` |
| [008](008/) | 同上 | 同上 | `gap_x=244, gap_y=2` |
| [012](012/) | 同上 | 同上 | `gap_x=230, gap_y=2` |

> 答案空间是一维水平位移；`gap_y` 在 12 组中只有 1–2，说明模板纵向位置基本固定，求解重点是把水平边缘对齐。

## 三、样本长什么样

| 背景题面 | 拼图块（alpha） | 拼合结果 |
|---|---|---|
| <img src="001/bg.jpg" width="240" alt="易盾滑块拼图 001 背景"> | <img src="001/aux-01.png" width="90" alt="易盾滑块拼图 001 拼图块"> | <img src="001/solved.png" width="240" alt="易盾滑块拼图 001 拼合结果"> |
| <img src="008/bg.jpg" width="240" alt="易盾滑块拼图 008 背景"> | <img src="008/aux-01.png" width="90" alt="易盾滑块拼图 008 拼图块"> | <img src="008/solved.png" width="240" alt="易盾滑块拼图 008 拼合结果"> |
| <img src="012/bg.jpg" width="240" alt="易盾滑块拼图 012 背景"> | <img src="012/aux-01.png" width="90" alt="易盾滑块拼图 012 拼图块"> | <img src="012/solved.png" width="240" alt="易盾滑块拼图 012 拼合结果"> |

- 第 2 列是带 alpha 的拼图块；透明区必须在匹配时屏蔽，否则会把块外背景当成特征。
- 第 3 列是把拼图块贴回缺口后的效果；008 的缺口靠右（`gap_x=244`），是本批最靠右的一组。

## 四、形态与难点

- **图幅固定**：320×160 背景，60×158 拼图块；缺口是细长条，横向边缘弱。
- **缺口与拼图块颜色不一致**：缺口可能偏暗或被半透明遮罩覆盖，直接 RGB/SSD 匹配得分低。
- **纹理干扰**：背景照片的纹理、物体边缘和缺口边缘相似，容易在错误位置出现次峰。
- **纵向固定**：`gap_y` 仅 1–2px，纵向不必大范围搜索；重点是 x 方向的 1px 级对齐。

## 五、求解思路

源方案只比边缘结构：背景与拼图块都做 Sobel 梯度，用 alpha 掩膜屏蔽透明区，再沿 x 轴做归一化互相关，取峰值。

| 步骤 | 做法 | 关键判据 / 参数 |
|---|---|---|
| 1 预处理 | 转灰度；拼图块取 alpha 掩膜 | 透明区不参与匹配 |
| 2 边缘 | 两侧分别做 Sobel，取梯度幅值 | 保留边缘强弱，弱化颜色差异 |
| 3 匹配 | 以固定 y 对齐，沿 x 滑动做 masked NCC | 步长 1px；取全局峰值 |
| 4 输出 | 峰值对应的 x 为 `gap_x`，y 为 `gap_y` | 12 组全部 high |

```text
# 缺口定位（伪代码，对应源方案 masked 梯度 + NCC）

g_bg   = sobel_magnitude(gray(bg))          # 320 x 160
g_tile = sobel_magnitude(gray(tile))        # 60 x 158
alpha  = alpha_channel(tile)

best_x, best_score = 0, -inf
for x in 0 .. (W - w):
    patch = g_bg[y0 : y0+h, x : x+w]
    score = masked_ncc(g_tile, patch, mask=alpha)
    if score > best_score:
        best_x, best_score = x, score

answer = (best_x, y0)                        # gap_x, gap_y
```

> 直接 RGB 匹配在缺口偏暗时得分很低；Sobel 只保留边缘强弱，alpha 掩膜把拼图块外的背景排除，相关峰才会落在缺口内部结构上。

## 六、结果与审查

- **源记录**：12/12 离线求解；本库 12 组 `result.json` 的置信度均为 high。
- **本库**：12 组 `answer.txt` / `result.json` / `aux-01.png` / `solved.png` 齐全，`evidence: offline-verified`。
- **人工审查**：12 组 `review.state=pass`、`result=correct`（核对缺口坐标；不含轨迹与服务端接受率）。

| 组 | gap_x | gap_y | 审查 |
|---|---:|---:|---|
| [001](001/) | 89 | 2 | pass |
| [002](002/) | 87 | 2 | pass |
| [003](003/) | 155 | 2 | pass |
| [004](004/) | 104 | 2 | pass |
| [005](005/) | 68 | 1 | pass |
| [006](006/) | 85 | 1 | pass |
| [007](007/) | 73 | 1 | pass |
| [008](008/) | 244 | 2 | pass |
| [009](009/) | 231 | 2 | pass |
| [010](010/) | 230 | 1 | pass |
| [011](011/) | 196 | 1 | pass |
| [012](012/) | 230 | 2 | pass |

## 七、失败与边界

| 现象 | 根因 | 证据 | 结论 |
|---|---|---|---|
| 直接 RGB 匹配得分低 | 缺口偏暗、拼图块是彩色，像素分布不一致 | 源方案早期试验 | 改用 Sobel 梯度 + alpha mask |
| 错误位置出现次峰 | 背景纹理与缺口边缘相似 | 12 组中有弱纹理样本 | 取全局峰并保留置信度；必要时人工复核 |
| 缺口靠近右边界 | 搜索范围接近 `W-w` | 008/009/010/012 的 `gap_x` 在 230–244 | 搜索上界用 `W-w`，不要少搜尾部 |
| 无线上提交记录 | 未做轨迹与提交闭环 | 统一标 `offline-verified` | 离线坐标正确 ≠ 线上通过 |

**边界**：本页只覆盖缺口定位；真实提交还需要轨迹、时序与风控环境，不在本库验证范围。

## 八、相关

- 厂商索引：[docs/yidun.md](../../../docs/yidun.md)
- 全量类型：[docs/types.md](../../../docs/types.md) · 方法论：[docs/00-methodology.md](../../../docs/00-methodology.md)
- 同厂商对照：[inference](../inference/)（8 块换位）、[slide-rotate](../slide-rotate/)（旋转拼图）、[icon-click](../icon-click/)（图标点选）
- 案例：[001](001/) – [012](012/)（每组含 `meta.json` / `answer.txt` / `result.json` / `aux-01.png` / `solved.png`）
- 来源：<https://dun.163.com/trial>；样本版权归网易易盾所有，仅用于研究与教学。
