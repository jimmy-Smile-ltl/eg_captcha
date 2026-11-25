# 整理规则

本库只收三类东西：**初始样本、最终求解结果、求解思路**。规则的目标是让结构统一、可核对、不夹带工具。

## 一、无可执行代码

本库不收录可运行的求解器、脚本、命令或 SDK；`tests/`、生成器、检查脚本一律放在仓库外。文档中的**伪代码/公式/参数链路**是知识表达，不是可执行实现：只允许 `text` 或无语言标记的代码块，禁止 `python`、`js`、`sh` 等语言标签，禁止 `import`、`pip`、`npm`、`curl` 等真实命令。

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

文档中允许下列**不可执行**内容：目录树、链路示意、伪代码、算法公式、参数解码过程。伪代码必须是对现有求解记录的直译，不写完整可运行函数、真实命令、密钥或 token；语言标记只用 `text`。

`skill/` 目录只放 Markdown（`SKILL.md` + `references/`），不放求解器脚本、密钥、token 或可执行代码；它只是知识库的索引、方法选型与审查流程。开源版不包含个人绝对路径，路径由 `EG_CAPTCHA_ROOT` 或 skill 相对位置确定。

## 二、目录与命名

```
cases/<vendor>/<type>/
├── README.md               # 类型页：题面 / 样本 / 思路 / 伪代码 / 结果 / 失败
└── <NNN>/                  # 单组案例：meta.json + 媒体 + 可选答案/结果
```

- `<vendor>`：小写英文 slug，见下表。
- `<type>`：受控主 slug，见下表；变体写进 `meta.json`，不建子目录。
- `<NNN>`：三位编号，先精选后编号，入库后不重排、不复用；默认每类最多 12 组，超过时按多样性精选（例外：`qingtongbu/curve` 24 组 = 旧 12 + 新 12，`shumei/icon-click` 14 组）。
- `README.md`：每个有案例目录的类型必须有且只有一个 `.md`；图片引用同目录相对路径，案例链接写 `[001](001/)`。

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
| `baidu` | 百度（爱企查 wappass） |
| `bilibili` | 哔哩哔哩 B站（极验实现） |

### 类型 slug（主名 + 别名）

| 主 slug | 中文 | 收编别名 |
|---|---|---|
| `slide` | 滑动拼图 | slider, slider2, slider_71, slider_163 |
| `slide-no-image` | 无图滑块 | slide-no-image |
| `slide-inference` | 滑动推理 | sliderinf_202 |
| `slide-restore` | 滑动还原 | concat, slide-restore |
| `slide-rotate` | 旋转拼图 | jigsaw_new |
| `jigsaw` | 拼图（易盾为缺口滑块） | jigsaw |
| `inference` | 8 块换位乱序拼图 | inference（易盾） |
| `rotate` | 旋转验证 | rotate |
| `angle` | 角度验证 | rotate_degree, angle |
| `curve` | 曲线滑动 | curve |
| `curve-ellipse` | 椭圆曲线 | curve2 |
| `curve-bezier` | 双贝塞尔曲线 | curve3 |
| `gesture` | 手势轨迹 | gesture |
| `scratch` | 刮刮卡 | scrape, scratch |
| `click` | 通用点选 | click |
| `icon-click` | 图标点选 | image_click, icon_130, icon |
| `text-click` | 文字点选 | text_click, picture-click, word_image_click |
| `text-click-enhanced` | 增强文字点选 | text_click_new |
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

> 天爱曲线按 API `type` 拆分：`CURVE`→`curve`、`CURVE2`→`curve-ellipse`、`CURVE3`→`curve-bezier`；三者绘制与参数机制不同，不能合并为变体。
>
> `random` 不是类型：体验页的「智能随机」只是从该厂商其他类型中随机返回一种，无独立题面；
> 不建 `cases/<vendor>/random/`，在厂商文档中说明即可。

## 三、单案例契约

固定文件：

| 文件 | 必需 | 内容 |
|---|---|---|
| `meta.json` | 是 | 来源、类型、变体、媒体、状态、求解器状态、审查状态 |
| `bg.*` | 图像类必需 | 原始主图，保持原字节 |
| `aux-01.*` | 可选 | 滑块块、提示图、第二画布等辅助图 |
| `restored.*` | 可选 | 打乱背景还原后的底图 |
| `solved.*` | 可选 | 最终结果 / 拼合效果图（源目录结果图，未复核） |
| `mark.*` | 可选 | 目标 / 拼图位置标记图 |
| `trajectory.*` | 可选 | 求解轨迹图 |
| `denoised.*` | 可选 | 去噪 / 去干扰后的题面或提示图（可含前后对照） |
| `split.*` | 可选 | 切分出的单字、图标或提示块（可含多块拼图） |
| `process-NN-*.*` | 可选 | 其他源求解器中间产物，按 `meta.process` 顺序展示 |
| `annotated.png` | 有结果时必需 | 答案可视化：绿=答案，黄=候选，红=错误/排除 |
| `answer.txt` | 有结果时必需 | 人类可读答案 |
| `result.json` | 有结果时必需 | 原样求解结果（脱敏） |

`status: sample-only` 的案例只放 `meta.json` + 媒体文件；没有可靠结果时不生成 `answer.txt` / `annotated.png` / `result.json`。`restored.*` / `solved.*` / `mark.*` / `trajectory.*` 是可视化过程图，不改变 `status`，也不代表已通过审查。

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
| `prompt` | 否 | **该组实际题面/操作提示**；优先从源数据字段或 hint 文件提取，不要只写类型级泛化描述 |
| `pass_criteria` | 否 | 通过条件/判定行为：点几个、拖到哪里、顺序要求、由服务端校验什么 |
| `roles` | 是 | 文件角色映射 |
| `process` | 否 | 源求解器过程图数组：`file` + `label`；按数组顺序展示 |
| `process_note` | 否 | 求解过程说明（无去噪/拆分产物时的处理方式也写在这里） |
| `answer_format` | 是 | `offset-x` / `points` / `angle` / `text` / `indices` / `swap` / `path` 等 |
| `method` | 是 | 方法标签：`CV` / `DET` / `OCR` / `VLM` / `ASR` / `PROTO` / `ACT` / `AUDIO` / `NONE`，可组合 |
| `method_note` | 是 | 是否用 LLM、成本/适用性、建议路线 |
| `status` | 是 | `sample-only` / `result-pending-review` / `solved`；**审查结论不写进 `status`**，只写 `review` |
| `solver.status` | 是 | `none` / `exists` / `weak` / `partial` / `browser` |
| `solver.accuracy` | 是 | `unknown` / `low` / `medium` / `high`（以源记录为准，不代表本库已复核） |
| `solver.notes` | 是 | 未撰写原因、准确率问题、提交闭环状态等 |
| `review.state` | 是 | `pending` / `not-applicable`；人工审查后改为 `pass` / `partial` / `fail` |
| `review.result` | 否 | 审查结论：`correct` / `partial` / `incorrect`，初始为空 |
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

每个类型必须在厂商索引的类型表与类型页中写明：

- 是否有求解器：`none` / `exists` / `weak` / `partial` / `browser`。
- 源记录准确率：`unknown` / `low` / `medium` / `high`；没有数据就写 `unknown`，不得猜。
- 无求解器的原因：未尝试、仅采集、优先级低、能力不足、未完成等；不知道原因就写“未撰写求解器”。
- 求解器差/无意义的原因：误报多、只对特定字体、服务端轨迹拒绝、提交闭环未打通等。
- 人工审查：`review.state` 初始 `pending`，结果留空；审查通过/不通过由维护者后续回填。

**纪律**：`sample-only` 不是“未解”的委婉说法，而是“样本已收、结果未纳入”的中性状态。文档不得把源求解器的自述准确率写成已复核结论。

## 六、文档规则

文档分两层：厂商索引 + 类型页。详细内容只写一处，避免两处漂移。

### 1. 厂商索引 `docs/<vendor>.md`（约 60–90 行）

1. **厂商概览**：来源、类型数/样本数、方法重点、一句话结论。
2. **8 列类型总表**：类型 / 中文 / 变体 / 样本 / 方法 / 求解器 / 审查 / 说明 + 类型页链接。列结构固定，第 4 列必须是纯数字；viewer 的 `build.mjs` 依赖该表识别 `sampleCount=0` 的 documented-only 类型。documented-only 行第 8 列用纯文本，不放 Markdown 链接。
3. **代表样本**：每类 1 张缩略图或音频链接，点击进类型页。
4. **厂商级结论**：强项、难点、已暴露的失败、证据边界。
5. **来源与版权**。

厂商索引不再放大段算法拆解、逐组答案表和失败档案；这些只在类型页。

### 2. 类型页 `cases/<vendor>/<type>/README.md`

固定八节：一、速览；二、题面与判定；三、样本长什么样；四、形态与难点；五、求解思路（C 档为“建议路线”）；六、结果与审查（C 档为“为什么没有结果”）；七、失败与边界；八、相关。A 档完整迁移深度拆解与逐组结果；B 档写步骤表与已知参数；C 档只写形态、建议路线、为何未解与边界，不写准确率。

写作要求：

- 中文；每节先用 1–2 句白话说明，再上表格/图；表格优先。
- 事实以案例 `meta.json` 为准；准确率标“源记录”；审查统计按 meta 现算；不把离线写成线上。
- 允许 `text` 伪代码/公式，仅当现有记录支持；不写可运行代码、命令、密钥、签名实现。
- 失败记录统一格式：**现象 → 根因 → 证据 → 结论**。
- 图片用相对路径 `<img src="001/bg.jpg" width="..." alt="...">`；禁止 script/style/iframe 和外链图片。
- 案例链接写 `[001](001/)`；指回厂商索引用 `../../../docs/<vendor>.md`。
- 图库选 4–8 张代表样本；全部案例由 viewer 按类型展示，正文只链接代表案例即可。

## 七、脱敏

必须剔除：cookie、token、session、签名、sitekey/secret、完整 query、原始 HTML/JSON 报文、代理 IP、UA、本地绝对路径、手机号/邮箱/账号/订单号、EXIF/GPS。

保留：公开来源 URL（只到路径）、题面文本、尺寸、答案、置信度、耗时、失败结论。

## 八、自检

每个案例：

- [ ] `case_id` 与路径一致
- [ ] `meta.json` 必填字段齐全，来源可公开访问
- [ ] `solver.status` / `solver.accuracy` / `review.state` 在枚举内
- [ ] `sample-only` 案例只有 `meta.json` + 媒体，无 `answer/annotated/result`
- [ ] `meta.process` 中的每个 `file` 都存在，且按求解顺序标注
- [ ] `result-pending-review` 案例的 `answer.txt` ↔ `annotated.png` ↔ `result.json` 数值一致
- [ ] 未解/无结果案例不伪造答案
- [ ] 无 token/cookie/个人信息/原始报文

类型页：

- [ ] 每个有案例的类型目录都有且只有一个 `README.md`，八节齐全
- [ ] 速览/结果中的样本数、求解器、审查统计与案例 `meta.json` 重算一致
- [ ] 图片、案例、厂商索引的相对链接全部存在
- [ ] 只用 `text` 或无语言标记的代码块；无 `python/js/sh` 标签、无真实命令
- [ ] 音频类型有音频链接或说明；无图类型不硬套图库

全库：

- [ ] 每个厂商的每个类型都有样本条目或 documented-only 条目
- [ ] 厂商索引保持 8 列类型表，第 4 列为纯数字；documented-only 行仍为 0
- [ ] 黑名单扩展名为 0
- [ ] 文档无可执行代码/命令
- [ ] 图片相对路径全部存在
- [ ] README 计数与目录实际一致
- [ ] 全库体积 ≤450MB（当前约 433MB）

## 九、外部项目并入流程

从 samples、其他仓库或临时采集目录并入本库时，按以下顺序执行，不能只搬图片或只写厂商表。

1. **素材归集**：只取公开来源的题面/媒体、答案/结果、过程图、标注；剔除源码、模型权重、原始报文、cookie/token、调试产物与个人信息。保留 `source_url` 与采集日期。
2. **案例落地**：`cases/<vendor>/<type>/<NNN>/` + `meta.json` + 媒体；有结果时放 `answer.txt` / `result.json` / `annotated.png`；`sample-only` 只放 meta+媒体，不伪造答案。
3. **类型页**：写 `cases/<vendor>/<type>/README.md` 八节；A/B/C 分档；4–8 张图；统计从 `meta.json` 现算；准确率标「源记录」；失败写「现象 → 根因 → 证据 → 结论」；伪代码只 `text`。
4. **厂商索引**：`docs/<vendor>.md` 保持 8 列类型表（第 4 列纯数字；documented-only 行写 0 且第 8 列纯文本）；第 8 列链到类型页；补代表样本、厂商级结论、来源。
5. **全局索引**：`docs/types.md` 厂商覆盖链到类型页；新厂商补 README 厂商总览、`docs/demos.md`、NOTICE 来源、本文厂商 slug 表；重算 README 的厂商/类型/案例/审查计数。
6. **skill**：更新 `skill/eg-captcha/references/repo-map.md` 的厂商表、类型页路径与快照统计；`SKILL.md` 路由保持「厂商索引 → 类型页 → meta.json」。
7. **校验**：仓库外脚本检查 README 覆盖、链接/图片、8 列表格、代码围栏；用 meta 重算样本数与审查统计。
8. **viewer**：在 viewer 仓库重新构建（命令与数据路径解析见该仓库 README），确认厂商数 / 厂商×类型数 / 案例数符合预期（当前 18 / 93 / 995，其中 7 个 documented-only），并打开类型页与案例页检查布局。

**外部并入检查清单**：

- [ ] 新增/变更的类型都有 `README.md`，且没有第二个 `.md`
- [ ] 厂商索引 8 列表格第 4 列与案例目录数一致；documented-only 仍为 0
- [ ] `docs/types.md` 链接数 = 有案例的厂商×类型数（当前 86）
- [ ] 案例媒体/答案/结果没有被外部项目的临时文件、模型或原始报文污染
- [ ] viewer 构建输出与 README/skill 统计一致
- [ ] 三个仓库已按「强制同步规则」一起更新

**不要**：把外部项目目录结构原样搬入；把厂商索引写成第二个类型页；为 documented-only 类型建空案例目录/README；用改 `status` 的方式表达审查结论。
