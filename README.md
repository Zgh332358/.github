# 共鸣 · StepFun 接入分支

这是 [revoice-resonance](https://github.com/revoice-resonance) 各项目在 **Zgh332358** 账号下的 fork 索引。原项目许可证和来源保持不变；本账号维护 StepFun 云端多模态与语音适配。

## 仓库与职责

| 仓库 | 职责与接入范围 |
| --- | --- |
| [web-app](https://github.com/Zgh332358/web-app) | React 前端与 Worker 代理；录音转写、朗读、文本/图片理解 API；可作为基础试用入口 |
| [backend](https://github.com/Zgh332358/backend) | 独立 Go 任务后端，负责云端识别/合成任务、音频存储和语料接口 |
| [asr](https://github.com/Zgh332358/asr) | Python 转写服务，保留语料与队列入口，替换本地 Whisper 推理路径 |
| [ios-app](https://github.com/Zgh332358/ios-app) | iOS WebView 外壳；配置本账号网页部署地址，供应商 Key 仍由网页后端保管 |
| [.github](https://github.com/Zgh332358/.github) | 本索引与 fork 说明 |

原组织的 [tts](https://github.com/revoice-resonance/tts) 截至 2026-10-02 没有 Git 内容，GitHub 拒绝 fork 空仓库。当前合成适配在 `web-app` 与 `backend`，没有为该空仓库虚构代码或 fork 关系。

## 模型与配置

官网 HTTP base URL 为 `https://api.stepfun.com/v1`。模型名必须按接口区分：

| 用途 | 模型 | 上游接口 |
| --- | --- | --- |
| 文本与图片理解 | `step-5-preview` | `/chat/completions` |
| 录音输入、文字转写 | `stepaudio-3-chat-preview` | `/chat/completions`，`input_audio` |
| 文字朗读 | `stepaudio-3-tts` | `/audio/speech` |

语音 Chat Preview 用于当前逐次录音流程；它不是专用 ASR 模型，严格转写提示不保证逐字忠实。`stepaudio-3-realtime-preview` 是另一套 WebSocket 实时会话协议，本分支没有实现持续双向实时通话。多模态接入提供 API，不包含新的图片聊天界面。

服务端使用 `OPENAI_API_KEY`、`OPENAI_BASE_URL`，并按职责使用 `MULTIMODAL_MODEL`、`ASR_MODEL`、`TTS_MODEL` 和 `TTS_VOICE`。示例 Key 留空，由运行者自行配置；不能放入前端构建变量或 iOS 应用包。

优先从 [web-app 的 README](https://github.com/Zgh332358/web-app#readme) 开始。其他仓库按各自 README 独立运行，Web 前端默认使用自身 Worker，不会自动串联全部后端。每个仓库的 `docs/specs/stepfun-preview.md` 记录对应范围。

## 验证范围

各仓库的 README 记录本次实际检查及尚缺条件。模拟上游与代码测试用于验证协议、错误和流程；未提供真实 Key 前，不声明 StepFun 实际推理、音色权限或目标用户识别效果已经验证。语料存储不会自动训练或改善模型；也不把基础版本写成任何识别率承诺。

推送到本账号 fork 不代表部署完成。云端发布或 iOS 签名打包需要另外配置，并明确执行相应部署流程。

官方参考：[Step 5 Preview](https://platform.stepfun.com/docs/zh/guides/models/step-5-preview)、[StepAudio 3](https://platform.stepfun.com/docs/zh/guides/models/stepaudio-3-realtime)、[语音合成 API](https://platform.stepfun.com/docs/zh/api-reference/audio/create-audio)。
