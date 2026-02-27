<img width="1605" height="1546" alt="image" src="https://github.com/user-attachments/assets/0da37a97-822a-4113-abeb-00f80ff6d6f2" />  

# 🎨 Nano-Banana AI 图像生成器

这是一个基于 HTML5 的轻量级单文件 Web 应用，专门用于通过 API 调用 **Gemini** 或 **Nano-banana-2** 模型进行 AI 图像生成。它提供了直观的界面来处理多图参考生成任务。

### ✨ 主要功能

* **多图参考生成**：支持最多 14 张参考图片，可用于角色一致性、风格迁移或场景融合。
* **智能引用系统**：在提示词框中输入 `@` 即可弹出引用面板，快速在提示词中关联特定参考图（如：`让 @img1 的人物穿上 @img2 的衣服`）。
* **便捷图片导入**：支持点击上传、拖拽上传，以及最方便的**剪贴板粘贴 (Ctrl+V)** 快速导入参考图。
* **参数深度自定义**：
* 支持多种宽高比（1:1, 9:16, 16:9, 21:9 等）。
* 针对 `nano-banana-2` 模型支持分辨率选择（1K/2K/4K）。
* 可切换 API 返回格式（URL 或 Base64 JSON）。


* **本地配置管理**：支持 API Base URL、API Key 和模型名称的本地保存、导入与导出（JSON 格式）。
* **生成历史记录**：自动保存最近的生成记录，支持预览、放大查看及一键下载。
* **响应式设计**：基于 Tailwind CSS 构建，适配桌面端和移动端，配备优雅的深色模式与玻璃拟态界面。

### 🚀 快速开始

1. **打开文件**：直接在浏览器中打开 `comfly-api调用简易app.html`。
2. **配置 API**：在顶部面板输入你的 API Base URL、API Key 和模型名称，点击“保存配置”。
3. **添加参考图**：将鼠标悬停在左侧的图片槽位上并按 `Ctrl+V`，或点击槽位上传。
4. **编写提示词**：在提示词框描述你的创意，使用 `@` 引用已上传的图片。
5. **生成**：点击“开始生成”，等待 AI 创作完成。

---

# 🎨 Nano-Banana AI Image Generator

A lightweight, single-file HTML5 web application designed for AI image generation via **Gemini** or **Nano-banana-2** model APIs. It provides an intuitive interface tailored for multi-image reference generation tasks.

### ✨ Key Features

* **Multi-Image Reference**: Supports up to 14 reference images, ideal for character consistency, style transfer, or scene blending.
* **Smart Mention System**: Simply type `@` in the prompt editor to trigger a popup for quickly referencing specific images (e.g., `Put the character from @img1 in the outfit from @img2`).
* **Effortless Image Import**: Supports click-to-upload, drag-and-drop, and seamless **Clipboard Paste (Ctrl+V)** for rapid image loading.
* **Deep Customization**:
* Multiple aspect ratios supported (1:1, 9:16, 16:9, 21:9, etc.).
* Resolution selection (1K/2K/4K) for supported models like `nano-banana-2`.
* Toggleable API response formats (URL or Base64 JSON).


* **Local Config Management**: Save, import, or export your API settings (Base URL, Key, Model Name) as JSON files.
* **Generation History**: Automatically tracks recent creations with features for previewing, zooming, and one-click downloading.
* **Responsive UI**: Built with Tailwind CSS, featuring a sleek dark-themed glassmorphism design that works on both desktop and mobile.

### 🚀 Quick Start

1. **Open File**: Launch `comfly-api调用简易app.html` directly in any modern web browser.
2. **Configure API**: Enter your API Base URL, API Key, and Model Name in the top panel, then click "Save Config".
3. **Add Images**: Hover over an image slot and press `Ctrl+V`, or click the slot to upload reference photos.
4. **Write Prompt**: Describe your vision in the prompt box, using `@` to link your uploaded references.
5. **Generate**: Click "Start Generation" and wait for the AI to bring your ideas to life.
