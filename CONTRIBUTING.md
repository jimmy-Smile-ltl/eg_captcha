# 整理规则

本库只收三类东西：**初始样本、最终求解结果、求解思路**。规则的目标是让结构统一、可核对、不夹带工具。

## 一、零代码

### 允许的文件

| 类别 | 扩展名 |
|---|---|
| 文档 | `.md` |
| 数据 | `.json` `.txt` |
| 图片 | `.png` `.jpg` `.jpeg` `.webp` `.gif` |
| 音频 | `.mp3` `.wav` `.aac` `.m4a` |

### 禁止的文件

| 类别 | 例子 |
|---|---|
| 代码 | `.py .js .ts .java .go .sh .ps1 .html .css .wasm` |
| 模型/权重 | `.onnx .pt .pth .pb .h5 .pkl` |
| 可执行/压缩包 | `.exe .dll .so .jar .zip .7z` |
| 原始报文 | `cookies.json`、`probe.json`、`load_response.json`、`raw_*`、`challenge.js`、`frame.html`、日志 |
| 训练/评估产物 | `yolo_dataset/`、`runs/`、`weights/`、`negative/`、`contact_sheet*` |

文档中不得出现可执行代码或命令片段；只允许无语言标记的纯文本目录树/链路示意。

`skill/` 目录只放 Markdown（`SKILL.md` + `references/`），不放求解器脚本、密钥、token 或可执行代码；它只是知识库的索引、方法选型与审查流程。开源版不包含个人绝对路径，路径由 `EG_CAPTCHA_ROOT` 或 skill 相对位置确定。

## 二、目录与命名

```
cases/<vendor>/<type>/<NNN>/
```

- `<vendor>`：小写英文 slug，见下表。
- `<type>`：受控主 slug，见下表；变体写进 `meta.json`，不建子目录。
- `<NNN>`：三位编号 `001`–`012`，先精选后编号，入库后不重排、不复用。

### 厂商 slug

| slug | 厂商 |
|---|---|
| `geetest` | 极验 GeeTest |
| `yidun` | 网易易盾 |
| `tencent` | 腾讯云验证码 |
| `shumei` | 数美 |
| `dingxiang` | 顶象 |
| `tianai` | 天爱 |
| `cup` | 中国银联 |
| `qingtongbu` | 企讯通 |
| `kgcaptcha` | 凯格 |
| `meituan` | 美团 Yoda |
| `amazon` | Amazon AWS WAF |
| `recaptcha` | Google reCAPTCHA |
| `hcaptcha` | hCaptcha |
| `human-security` | HUMAN Security |
| `datadome` | DataDome |
| `mtcaptcha` | MTCaptcha |

### 类型 slug（主名 + 别名）

| 主 slug | 中文 | 收编别名 |
|---|---|---|
| `slide` | 滑动拼图 | slider, slider2, slider_71, slider_163 |
| `slide-no-image` | 无图滑块 | slide-no-image |
| `slide-inference` | 滑动推理 | sliderinf_202 |
| `slide-restore` | 滑动还原 | concat, slide-restore |
| `slide-rotate` | 旋转拼图 | jigsaw_new |
| `jigsaw` | 乱序拼图 | inference, jigsaw |
| `rotate` | 旋转验证 | rotate |
| `angle` | 角度验证 | rotate_degree, angle |
| `curve` | 曲线滑动 | curve, curve2, curve3 |
| `gesture` | 手势轨迹 | gesture |
| `scratch` | 刮刮卡 | scrape, scratch |
| `click` | 通用点选 | click |
| `icon-click` | 图标点选 | image_click, icon_130, icon |
| `text-click` | 文字点选 | text_click, text_click_new, picture-click, word_image_click |
| `word-order` | 语序点选 | word_order_image_click, word-order |
| `word-group` | 语序选词 | word-group |
| `scene-click` | 场景语义点选 | scene-click |
| `diff-click` | 差异点击 | diff-click |
| `area` | 面积验证 | area |
| `area-select` | 区域点选 | hcaptcha area_select* |
| `spatial` | 空间推理 | space-inference |
| `font` | 字体识别 | font |
| `line-connect` | 最短线连点 | sudoku_103 |
| `avoid` | 障碍躲避 | avoid |
| `text` | 字符识别 | image_1, mtcaptcha |
| `grid` | 九宫格多选 | grid_5 |
| `grid-independent` | 独立子图九宫格 | reCAPTCHA independent |
| `grid-split` | 整图切分九宫格 | reCAPTCHA split |
| `binary` | 九宫格二分类 | hcaptcha binary* |
| `drag-drop` | 拖拽拼块 | hcaptcha drag_drop* |
| `gobang` | 五子棋 | gobang |
| `icon-crush` | 图标消除 | iconcrush |
| `voice` | 语音验证 | voice |
| `audio` | 音频听写 | audio-6digit |
| `pow` | 工作量证明 | pow |
| `token` | 指纹 / Token 挑战 | token |
| `invisible` | 无感 | invisible, sense |
| `press-hold` | 按压 | press-hold |
| `accessible-email` | 无障碍邮件码 | accessible-email |
| `sms` | 短信上行 | sms |
| `random` | 智能随机 | random |

## 三、单案例契约

固定文件：

| 文件 | 必需 | 内容 |
|---|---|---|
| `meta.json` | 是 | 来源、类型、变体、媒体、状态、求解器状态、审查状态 |
| `bg.*` | 图像类必需 | 原始主图，保持原字节 |
| `aux-01.*` | 可选 | 滑块块、提示图、第二画布等辅助图 |
| `annotated.png` | 有结果时必需 | 答案可视化：绿=答案，黄=候选，红=错误/排除 |
| `answer.txt` | 有结果时必需 | 人类可读答案 |
| `result.json` | 有结果时必需 | 原样求解结果（脱敏） |

`status: sample-only` 的案例只放 `meta.json` + 媒体文件；没有可靠结果时不生成 `answer.txt` / `annotated.png` / `result.json`。

分支：

- 音频类：`audio.*` + `answer.txt`，无 `annotated.png` 时在 meta 说明。
- 无图/协议类：只放 `meta.json` + `answer.txt`（有答案时），不建媒体文件。
- 动画类：存 `bg.*`（关键帧）+ 可选第二帧 `aux-01.*`，不存视频。
- 未解：无 `answer.txt`/`annotated.png`，`status: unsolved`。

### `meta.json` 字段

| 字段 | 必填 | 说明 |
|---|---|---|
| `case_id` | 是 | `<vendor>/<type>/<NNN>`，与路径一致 |
| `vendor` / `vendor_name` | 是 | slug / 中文名 |
| `type` / `type_name` | 是 | 主 slug / 中文名 |
| `variant` | 否 | 版本、厂商 type code、题面族 |
| `media` | 是 | 数组：`image` / `audio` / `video`；无媒体为 `[]` |
| `source_url` | 是 | 公开来源 URL，不带 token/query |
| `source_path` | 否 | 源相对路径，便于复核 |
| `collected_at` | 是 | `YYYY-MM-DD` |
| `prompt` | 否 | 题面/操作提示 |
| `roles` | 是 | 文件角色映射 |
| `answer_format` | 是 | `offset-x` / `points` / `angle` / `text` / `indices` / `swap` / `path` 等 |
| `method` | 是 | 方法标签：`CV` / `DET` / `OCR` / `VLM` / `ASR` / `PROTO` / `ACT` / `AUDIO` / `NONE`，可组合 |
| `method_note` | 是 | 是否用 LLM、成本/适用性、建议路线 |
| `status` | 是 | `sample-only` / `result-pending-review` / `reviewed-pass` / `reviewed-fail` |
| `solver.status` | 是 | `none` / `exists` / `weak` / `partial` / `browser` |
| `solver.accuracy` | 是 | `unknown` / `low` / `medium` / `high`（以源记录为准，不代表本库已复核） |
| `solver.notes` | 是 | 未撰写原因、准确率问题、提交闭环状态等 |
| `review.state` | 是 | `pending` / `not-applicable`；人工审查后改为 `pass` / `fail` |
| `review.result` | 否 | 审查结论，初始为空 |
| `review.reviewed_at` | 否 | 审查日期，初始为空 |
| `notes` | 否 | 弱标签、取舍、失败原因 |

`solver.status` 含义：

| 值 | 含义 |
|---|---|
| `none` | 没有撰写求解器（未尝试/未完成/仅采集样本） |
| `exists` | 有求解器与结果，准确率待人工审查 |
| `weak` | 有求解器，但准确率低或误报多，参考价值有限 |
| `partial` | 部分进展：图像/答案已解但提交未通，或只完成前置还原/检测 |
| `browser` | 只能靠真实浏览器交互，纯协议不可行 |

`review.state` 初始为 `pending`（有求解器/待审）或 `not-applicable`（无求解器）；审查结果由维护者后续回填，不得预设。

方法标签必须写清“是否借助多模态 LLM / 视觉推理 / 物体识别”：纯 CV/OCR/检测就标对应标签；用了视觉大模型做视觉推理就标 `VLM`；混合方案用 `+` 连接（如 `DET+OCR+VLM`）。没有求解器时标 `NONE(建议路线)`。`method_note` 要说明成本：VLM 无需训练、按调用计费，起步便宜；量大时 CV 更便宜。

### `answer.txt` 格式

```
answer: gap_x=178, drag=178
unit: px
source: offline-verified
note: 缺口左上角坐标 (178, 96)，图为 320x160
```

按类型：滑块=缺口坐标/位移；点选=有序坐标；旋转=角度；字符=文本；排序=排列；网格=格子编号；音频=听写文本。

## 四、覆盖优先与 12 组规则

**先覆盖，后求解。** 每个厂商的每个类型都必须有样本条目或 documented-only 条目，不能因为“没有求解器/求解器差”而漏收。样本是中立的，求解器状态单独记录。

1. 每个（厂商, 类型）最多 12 组；不足 12 组全收；超过 12 组按多样性精选。
2. 覆盖批次优先保证“每个类型至少 1 组”；没有可靠结果的类型收 `sample-only`，不强行补答案。
3. 有结果的类型优先收高置信/可复核案例，其余作为补充。
4. 按变体/题面族/媒体形态分层，保证多样性。
5. 近似重复只留 1 组。
6. 源目录为 `sets/NN/`、`NN/`、`live/rounds/NN`、`samples/<批次>` 时，以“一个可独立成题的目录/文件簇”为单位。

## 五、求解器状态与人工审查

每个类型必须在厂商文档的类型表中写明：

- 是否有求解器：`none` / `exists` / `weak` / `partial` / `browser`。
- 源记录准确率：`unknown` / `low` / `medium` / `high`；没有数据就写 `unknown`，不得猜。
- 无求解器的原因：未尝试、仅采集、优先级低、能力不足、未完成等；不知道原因就写“未撰写求解器”。
- 求解器差/无意义的原因：误报多、只对特定字体、服务端轨迹拒绝、提交闭环未打通等。
- 人工审查：`review.state` 初始 `pending`，结果留空；审查通过/不通过由维护者后续回填。

**纪律**：`sample-only` 不是“未解”的委婉说法，而是“样本已收、结果未纳入”的中性状态。文档不得把源求解器的自述准确率写成已复核结论。

## 六、文档规则

每家厂商一篇：`docs/<vendor>.md`。覆盖批次统一为四段：

1. **类型总表**：类型 / 中文 / 变体 / 样本 / 求解器 / 准确率（源记录）/ 审查 / 说明。
2. **样本速览**：每类 1 张代表样本，完整样本留在 `cases/`。
3. **求解器与人工审查**：按 `none` / `weak` / `partial` / `browser` / `exists` 分组，写清无求解器原因或效果差原因。
4. **来源**：公开来源、采集日期、版权说明。

深度拆解（形态 / 识别思路 / 提交链路 / 失败记录）作为可选附录，成熟后再补；极验文档是深度样板的例外。

写作要求：

- 中文；表格优先；不写代码、命令、密钥、签名实现、完整加密步骤。
- 准确率必须标为“源记录”，并注明日期或来源；没有日期就写 `unknown`，不得写成已复核。
- 失败记录统一格式：**现象 → 根因 → 证据 → 结论**。
- 图库用相对路径的图片；尺寸控制仅允许 `<img src="..." width="..." alt="...">`，禁止 script/style/iframe。

## 七、脱敏

必须剔除：cookie、token、session、签名、sitekey/secret、完整 query、原始 HTML/JSON 报文、代理 IP、UA、本地绝对路径、手机号/邮箱/账号/订单号、EXIF/GPS。

保留：公开来源 URL（只到路径）、题面文本、尺寸、答案、置信度、耗时、失败结论。

## 八、自检

每个案例：

- [ ] `case_id` 与路径一致
- [ ] `meta.json` 必填字段齐全，来源可公开访问
- [ ] `solver.status` / `solver.accuracy` / `review.state` 在枚举内
- [ ] `sample-only` 案例只有 `meta.json` + 媒体，无 `answer/annotated/result`
- [ ] `result-pending-review` 案例的 `answer.txt` ↔ `annotated.png` ↔ `result.json` 数值一致
- [ ] 未解/无结果案例不伪造答案
- [ ] 无 token/cookie/个人信息/原始报文

全库：

- [ ] 每个厂商的每个类型都有样本条目或 documented-only 条目
- [ ] 黑名单扩展名为 0
- [ ] 文档无可执行代码/命令
- [ ] 图片相对路径全部存在
- [ ] README 计数与目录实际一致
- [ ] 全库体积 ≤300MB
