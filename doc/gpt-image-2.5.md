# GPT Image 2.5 Sunburst / Flare（Comfly）

GPT Image 页签的「模型版本」新增 `gpt-image-2.5-sunburst` 和 `gpt-image-2.5-flare`，各自独立保存参数。两个模型依据根目录 `OpenAI Image.md`，复用现有 2.6 的扩展 GPT profile、官方 JSON 请求、任务快照、重试、历史和输出格式处理，不另建请求流程。

所有 GPT 模型（旧 GPT2、2.5 和 2.6）的客户端缺省尺寸为 `2048x1152`（2K 16:9），画质为 `high`。两个 2.5 模型的其余可选参数缺省为 `none`。已保存的具体值，包括 `none`、`auto` 和自定义尺寸，均保持不变；默认值只补缺省字段，不强制迁移用户选择。

## Comfly 请求

使用 Comfly 自己的 API Key，默认 Base URL 为 `https://ai.comfly.org/v1`。所有提交使用 `Authorization: Bearer <Comfly API Key>` 与 `Content-Type: application/json`。

- 无参考图：`POST /v1/images/generations?async=true`。
- 有参考图：`POST /v1/images/edits?async=true`，以 `images: [{ "image_url": "data:image/...;base64,..." }]` 发送，最多 16 张。没有参考图时完全省略 `images`。
- 异步查询：`GET /v1/images/tasks/{task_id}`，使用相同 Comfly Key。

缺省文生图请求：

```json
{
  "model": "gpt-image-2.5-sunburst",
  "prompt": "一只像素风毛绒小恐龙",
  "size": "2048x1152",
  "quality": "high"
}
```

## 参数白名单

只构造 `model`、`prompt`、`images`、`size`、`quality`、`complexity`、`background`、`output_format`、`frame_count`、`frame_duration_ms`。所有非必选参数均提供字面值 `none`，表示不发送该字段；`auto` 表示显式发送 `auto`，两者不会互换。

- 尺寸支持 `none`、`auto`、页面像素预设和自定义 `WIDTHxHEIGHT`。
- 画质支持 `none/auto/low/medium/high/xhigh/max`；复杂度支持 `none/low/medium/high`。
- 背景支持 `none/auto/opaque/transparent`；格式支持 `none/png/jpeg/webp/gif/svg`。
- GIF 帧数支持 `none` 或 2 至 16 的整数；每帧时长支持 `none` 或至少 20 毫秒的整数。帧参数仅在 GIF 时显示、发送，切换其他格式仍保留原设置。
- 透明背景支持 PNG/WebP，格式为 `none` 时沿用默认 PNG。明确选择 JPEG/GIF/SVG 时提交前提示不兼容。
- SVG 不接受参考图，提交前明确提示，不静默丢图；不附加 `svg` 作者模型或推理对象。

隐藏且绝不发送：`output_compression`、`n`、`mask`、`markup`、`svg`（作者模型与推理）、`moderation`、`stream`、`partial_images`、`safety_identifier`、`user`、`input_fidelity`、`response_format`、`style`。不发送固定 `n=1`，也不发送旧式顶层 `image` 或 `url` 字段。

## 渠道支持与保存

这是客户端透传支持，不保证实际 Comfly 渠道已开放模型、全部质量等级、复杂度、透明背景、GIF/SVG 或全部尺寸。根文档对 2.5 的自定义尺寸列有 16 倍数、比例和像素边长等要求，具体有效组合及 GIF 预览权限仍以实际渠道为准。本文不提供未经核实的价格。

参数支持刷新、切换模型和配置导入导出。原任务参考图与完整结果继续保存在 IndexedDB，任务列表保留轻量元信息；重试沿用原模型、参数和图片。GIF/SVG 等输出按原 MIME 和格式保存、下载，不固定改成 JPG/PNG。详细行为与同目录 `gpt-image-2.6.md` 一致。
