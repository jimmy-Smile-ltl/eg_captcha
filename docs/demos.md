# 验证码体验入口

> 以下均为公开 demo / 体验页，用于观察交互与题面；请遵守站点条款，不要对真实业务站做未授权测试。第三方解决商的 demo 仅用于体验验证码形态，不代表本库推荐其服务，本库不提供其 API、密钥或调用代码。页面会随厂商改版/下线，以访问时为准。
>
> 返回 [README](../README.md)

## 官方公开 demo

| 厂商 | 入口 | 可体验类型 |
|---|---|---|
| 极验 GeeTest | <https://www.geetest.com/en/adaptive-captcha-demo> | no-captcha / slide / icon / gobang / iconcrush |
| 网易易盾 | <https://dun.163.com/trial/jigsaw>；综合页 <https://dun.163.com/trial/sense>；推理拼图 <https://dun.163.com/trial/inference> | 滑动拼图、无感、推理拼图等；其余类型按 `/trial/<type>` 替换（icon-click、word-order、word-group、space-inference、avoid、sms）。`/trial` 根路径可能 404 |
| 腾讯云验证码 | <https://cloud.tencent.com/product/captcha> | 页面“在线体验”：滑动拼图、文字点选、图形点选、无感、语音（以页面为准） |
| 数美 | <https://www.ishumei.com/new/product/tw/code> | 滑动拼图、文字点选、图标点选、语序点选、空间逻辑推理 |
| 顶象 | <https://www.dingxiang-inc.com/business/captcha> | 13 种验证方式 + 智能随机 |
| 天爱 | <https://captcha.tianai.cloud/> | 16 种（滑块/曲线/旋转/角度/点选/语序/刮刮卡/还原/手势/拼图/PoW 等） |
| 中国银联 | <https://captcha.cup.com.cn/> | 滑块、无图滑块、旋转、点选、场景语义点选；需点击“嵌入式”触发 |
| 企讯通 | <http://www.qingtongbu.com/#experience> | 滑动拼图、文字点选、图形点选、角度、旋转、曲线、智能随机 |
| 凯格 KgCaptcha | <https://www.kgcaptcha.com/> | 滑动拼图、文字点选、语序点选、字体识别、空间推理、智能随机 |
| MTCaptcha | <https://service.mtcaptcha.com/mtcv1/demo/> | 图片 + 语音双模态字符挑战 |
| hCaptcha | <https://accounts.hcaptcha.com/demo> | area-select / binary / drag-drop（服务端随机出题） |
| Google reCAPTCHA | v2 <https://www.google.com/recaptcha/api2/demo>；v3 <https://recaptcha-demo.appspot.com/recaptcha-v3-request-scores.php> | v2 勾选/图片挑战、音频挑战（耳机按钮切换）、v3 打分 |

## 官方产品页 / 真实挑战页（无公开 demo）

| 厂商 | 入口 | 说明 |
|---|---|---|
| Amazon AWS WAF | AWS WAF 文档：<https://docs.aws.amazon.com/waf/latest/developerguide/waf-js-captcha-api-render.html> | 无公开官方 demo；第三方 cap.guru 提供 AWS WAF 体验页。同一 widget 可用耳机图标在视觉九宫格与音频题之间切换 |
| DataDome | <https://datadome.co/products/captcha-alternative/> | 官方产品页，无公开 demo；本库有滑块/音频样本 |
| HUMAN Security | <https://www.humansecurity.com/platform/features/human-challenge/> | 官方产品页，无公开 demo；本库有按压/无感截图 |
| 美团 Yoda | <https://verify.meituan.com/v2> | 真实业务挑战页，不是 demo；请勿做未授权测试 |

## 第三方验证码解决商 demo

| 服务商 | 入口 | 可体验类型 |
|---|---|---|
| Cap.guru | <https://test.cap.guru/demo/hcap#hcap2>（同站：<https://cap.guru/demo/hcap>） | hCaptcha 难度锚点：Easy `#hcap1` / Moderate `#hcap2` / Hard `#hcap3` / Discord `#hcap4`；另有 reCAPTCHA <https://cap.guru/demo/recap>、GeeTest <https://cap.guru/demo/geetest>、AWS WAF <https://cap.guru/demo/aws>、Cloudflare Turnstile <https://cap.guru/demo/cloud>、Arkose FunCaptcha <https://cap.guru/demo/funcap>、Text <https://cap.guru/demo/text> |
| 2Captcha | <https://2captcha.com/demo> | reCAPTCHA v2/v2 Invisible/v2 Enterprise/v3、GeeTest v3/v4、Cloudflare Turnstile、MTCaptcha、Normal/Click/Rotate/Text、Lemin、KeyCAPTCHA、SolveMedia。子页：<https://2captcha.com/demo/recaptcha-v2>、<https://2captcha.com/demo/recaptcha-v3>、<https://2captcha.com/demo/geetest-v4>、<https://2captcha.com/demo/cloudflare-turnstile>、<https://2captcha.com/demo/mtcaptcha>。hCaptcha 体验用 Cap.guru |
| CapMonster Cloud | <https://capmonster.cloud/en/demo/recaptcha-v3/> | reCAPTCHA v3；其他类型可在其 demo 目录查找 |

> Cap.guru 部分子页有 Anubis 访问校验，浏览器正常访问通常可见；若被拦截，稍后重试或改用 2Captcha 的对应 demo。
