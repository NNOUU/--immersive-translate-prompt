# 🚀 沉浸式翻译优化

本项目提供了一套为「[沉浸式翻译](https://immersivetranslate.com/)」插件配合自定义 API 的 Prompt 优化方案。通过底层逻辑优化，突破物理时间轴限制，免费实现高级版的**智能断句**与**专业级字幕排版**功能。

## ✨ 核心特性 (Features)

- **🧠 智能上下文**：打破 YouTube 自动生成字幕的机械切割，让 AI 根据上下文意群进行重组与意译。
- **⏱️ 时间轴对齐**：通过锁定 `%%` 分隔符数量，彻底解决多端重组导致的字幕渲染崩溃问题。
- **🧹 智能去干扰**：自动过滤 `[Music]`、`[Applause]` 等无意义的听障辅助标签，保持画面清爽。
- **📐 排版规范**：强制将所有中英文逗号、句号替换为空格，符合专业影视字幕组的视觉标准。

---

## ⚙️ 最佳参数配置 (Configurations)

在使用自定义 Prompt 前，请务必在插件的 API 设置页完成以下参数调整：
以 Deepseek 为例

| 参数名称 | 推荐值 | 解析 |
| :--- | :---: | :--- |
| **每次请求最大段落数** | `8` 或 `10` | **必须修改**。扩大 AI 的上下文视野，是实现跨段落智能断句的物理前提。 |
| **Temperature (发散度)**| `0.8` ~ `1.3` | **视频字幕场景推荐**。温度需求视选择的模型而定。 |
| **每次请求最大文本长度**| `1200` | 日常看视频足够。若需兼顾大量网页长文翻译，可上调至 `1500~2000`。 |

> **💡 提示**：对于纯文本阅读（如长篇博客），DeepSeek 官方推荐的 Temperature 为 `1.3`，意译效果极佳。建议在插件中为“网页翻译”和“视频翻译”创建两个独立的 DeepSeek 服务配置。

---

## 📜 核心 Prompt 部署 (The Core Prompt)

请进入插件设置 -> **翻译服务** -> 找到 **Subtitle Prompt**，将默认内容替换为以下完整代码：

```text
Translate the following sequential subtitles into {{to}}.

CRITICAL INSTRUCTIONS FOR SUBTITLES (严格对齐、意译与去噪音):
1. Global Context: These are continuous subtitles. Read all segments first to grasp the complete meaning, then translate fluently.
2. EXACT Segment Match (CRITICAL): The input uses "%%" to separate subtitle segments. You MUST output the EXACT SAME number of segments, separated by "%%". DO NOT merge or delete any segments. 
3. Smart Distribution: If a single sentence is split across multiple input segments, figure out the full translation, then logically distribute the translated words across your output segments.
4. Remove Audio Tags (Noise Reduction): Completely ignore and remove any closed captioning audio tags enclosed in brackets or parentheses (e.g., [Music], [Laughter], (sigh)). Do NOT translate them. If a segment contains ONLY these tags, leave your translation for that segment completely empty, but YOU MUST STILL output the "%%" separator to maintain exact alignment.
5. Professional Punctuation: Replace ALL commas and periods (including half-width ", ." and full-width "， 。") in your final translation with a single space (" "). DO NOT output periods or commas.
6. NO Extra Text: Output ONLY the translated text and the "%%" separators. No markdown, no explanations.

{{text}}

🤝 参与贡献
如果你在使用过程中发现了更好的断句策略或参数组合，欢迎提交 Pull Request 或发起 Issue 讨论！
###
