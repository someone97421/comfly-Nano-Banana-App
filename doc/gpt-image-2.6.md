# GPT Image 2.6 Sunburst / Flare（Comfly）

本客户端在 GPT Image 页签的「模型版本」中提供 `gpt-image-2.6-sunburst` 和 `gpt-image-2.6-flare`。参数与 JSON 结构依据根目录 `OpenAI Image.md`，使用 Comfly 自己的 API Key 和 Base URL，默认 `https://ai.comfly.org/v1`。不需要 OpenAI Key。

这是客户端参数透传支持，不代表用户实际使用的 Comfly 渠道已支持两个模型、全部质量等级、透明背景、GIF 或 SVG。模型权限、GIF 预览权限、SVG 权限、尺寸限制及参数组合是否可用，须以实际渠道返回为准。本文不提供未经核实的价格。

## 请求与异步查询

全部请求使用 `Authorization: Bearer <Comfly API Key>`。提交请求的 `Content-Type` 为 `application/json`。

- 无参考图：`POST /v1/images/generations?async=true`。
- 有参考图：`POST /v1/images/edits?async=true`，采用官方 JSON `images` 数组。
- 查询：`GET /v1/images/tasks/{task_id}`，继续使用同一个 Comfly Key。

默认文生图请求（尚未保存尺寸与画质设置时）：

```json
{
  "model": "gpt-image-2.6-sunburst",
  "prompt": "一只像素风毛绒小恐龙",
  "size": "2048x1152",
  "quality": "high"
}
```

参考图编辑请求：

```json
{
  "model": "gpt-image-2.6-flare",
  "prompt": "参考输入图片的主体，生成新的海报",
  "images": [
    { "image_url": "data:image/png;base64,<参考图数据>" }
  ],
  "size": "2048x1152",
  "quality": "high",
  "complexity": "medium",
  "background": "opaque",
  "output_format": "png"
}
```

最多支持 16 张参考图。每个 `images` 元素只发送包含 MIME 的 data URL；没有参考图时完全省略 `images`。不发送旧 GPT2 使用的顶层 `image`、`url` 或 `response_format`。参考图片继续沿用客户端原有的本地图片处理流程，WebP 等输入可能转换为 JPG。

## 开放参数与省略语义

除模型和提示词外，每个可选参数均提供字面值 `none`。选择 `none` 时请求中省略该字段；`none` 本身不会发送给渠道。`auto` 是实际发送的字符串，两者语义不同。所有 GPT 模型（旧 GPT2、2.5 和 2.6）的客户端缺省尺寸为 `2048x1152`（2K 16:9）、画质为 `high`；扩展模型的其余可选参数初始值为 `none`。已保存的具体值（包括 `none`、`auto`）保持不变，仅为缺省字段补默认值。

| 页面参数 | 请求字段 | 选项 / 规则 |
| --- | --- | --- |
| 模型版本 | `model` | `gpt-image-2.6-sunburst`、`gpt-image-2.6-flare` |
| 提示词 | `prompt` | 必填，1 至 32,000 个字符 |
| 参考图片 | `images` | 可不传，最多 16 张，每项为 `{ "image_url": "data:..." }` |
| 尺寸 | `size` | `none`、`auto`、页面预设或自定义 `WIDTHxHEIGHT` |
| 画质 | `quality` | `none`、`auto`、`low`、`medium`、`high`、`xhigh`、`max` |
| 复杂度 | `complexity` | `none`、`low`、`medium`、`high` |
| 背景 | `background` | `none`、`auto`、`opaque`、`transparent` |
| 输出格式 | `output_format` | `none`、`png`、`jpeg`、`webp`、`gif`、`svg` |
| GIF 帧数 | `frame_count` | `none` 或 2 至 16 的整数 |
| GIF 每帧时长 | `frame_duration_ms` | `none` 或至少 20 毫秒的整数 |

自定义宽高校验正整数；具体模型的像素、边长、比例限制及 GIF 尺寸限制由实际渠道校验。根目录官方参数文档未单独给出两个 2.6 模型的完整尺寸限制，因此不把旧 GPT2 的限制套到新模型上。页面沿用已有尺寸预设，预设可见不代表实际渠道支持该尺寸。

透明背景支持 `png` 或 `webp`；输出格式为 `none` 时不发送格式字段，沿用默认 PNG。明确选择不兼容格式时会保留用户原背景值，提交前报错，须主动修改背景或输出格式。

GIF 专属参数仅在显式选择 `gif` 时显示并发送。切换其他格式时保留已选 GIF 参数，但请求省略这两个字段。`frame_count` 省略时官方默认 9 帧；`frame_duration_ms` 省略时官方默认 200 毫秒。官方文档说明 GIF 会按 10 毫秒取整，并限制编码帧延迟，实际行为以渠道为准。

SVG 不接受参考图：存在参考图时提交前明确报错，不会静默丢弃图片。客户端不发送额外 `svg` 对象，也不发送固定的 `n=1`；服务端使用省略参数的默认行为。

## 隐藏且不发送的字段

两个新模型不显示、不构造、也不从配置中透传以下字段：

`output_compression`、`n`、`mask`、`markup`、`svg`（作者模型与推理设置）、`moderation`、`stream`、`partial_images`、`safety_identifier`、`user`、`input_fidelity`、`response_format`、`style`。

请求通过开放字段白名单构造。即使导入配置包含上述字段，也不会添加到新模型请求中。

## 参数保存、任务恢复与输出

- 两个新模型分别保存自己的参数，旧 GPT2 也保留自己的设置。
- 刷新页面、切换模型、配置导入导出保留 `none`、`auto`、自定义尺寸和 GIF 参数。
- 新任务以原模型、参数、Comfly 配置及参考图快照执行；参考图快照存入 IndexedDB，不放入配置导出文件。
- 新模型的已完成或失败任务提供「重试」，使用原任务参数和图片，不读取当前编辑区参数。刷新后仍可读取快照；快照丢失时明确报错。
- 有服务端任务 ID 的任务沿用现有异步恢复查询。没有任务 ID 的中断任务保持失败状态，可以手动重试，避免自动重复提交。
- 新模型历史记录保存生成参数与原任务参考图，复用时恢复模型和参数。旧历史记录仍可读取。
- GIF、SVG、JPEG、WebP、PNG 的 base64 结果按格式及文件签名识别 MIME；历史记录保存原始图片数据，不经画布转码，保留 GIF 动画和 SVG。
- 任务列表只在 localStorage 保存轻量元信息；新模型的完整结果另存 IndexedDB，刷新后恢复。存储失败不会中断当前队列；参考图快照缺失时仍保存生成图片。
- 下载文件名按 data URL MIME、URL 扩展名或任务返回格式确定后缀。远程图片能否保存到本地、跨域下载行为仍取决于渠道图片地址的 CORS 与浏览器行为。

旧 GPT2、Gemini 和 Seedream 的请求适配逻辑保留。本文的参数白名单、省略语义、官方 JSON edits、任务快照和输出处理也由两个 2.5 模型复用，详见同目录 `gpt-image-2.5.md`。
