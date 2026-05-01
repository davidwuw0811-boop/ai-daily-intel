# Open Design 项目研究报告

## 1. 项目核心定位与功能

**Open Design** 是一个开源的、本地优先的 AI 设计引擎，定位为 Anthropic 闭源产品 Claude Design 的开源替代方案 [1]。它不是一个独立的 AI 模型，而是一个将现有的强大编程 Agent（如 Claude Code、Cursor Agent、GitHub Copilot CLI 等）与设计工作流结合的框架。

该项目的核心功能在于其多 Agent 支持能力，能够自动检测并支持 11 种主流编程 Agent CLI，同时提供兼容 OpenAI 格式的 BYOK（自带密钥）代理。这种设计使得用户可以灵活选择底层模型，而不受单一厂商的限制。

在工作流方面，Open Design 采用技能驱动（Skill-driven）的架构。它内置了 31 种可组合的设计技能（Skills），涵盖原型设计、演示文稿、移动应用、仪表盘等多种场景。为了确保生成的 UI 具有专业级的一致性，项目还内置了 129 套设计系统（Design Systems），其中包括 70 套知名产品（如 Linear、Stripe、Vercel、Airbnb 等）的设计规范。

此外，Open Design 提供了基于 Web 的交互界面，支持实时查看 Agent 的 Todo 进度，并在沙盒 iframe 中即时预览生成的 HTML/CSS/JS 产物。在输出最终产物前，Agent 会根据设计哲学、层级、执行力、特异性和克制度五个维度对自身输出进行评分，低于 3 分则自动重构，从而保证了输出质量。

## 2. prompt-templates 目录分析

`prompt-templates` 目录包含了用于生成图像和视频的高质量提示词模板，分为 `image` 和 `video` 两个子目录。这些模板通过 JSON 格式定义了生成特定视觉内容所需的参数和结构化提示词。

### 图像模板 (Image Templates)
该目录共包含 43 个模板，主要涵盖头像/肖像、社交媒体海报以及插画与游戏资产等类别。

| 类别 | 典型模板示例 | 用途与核心内容 |
|---|---|---|
| **头像/肖像** | `profile-avatar-song-dynasty-hanfu-portrait.json` | 定义了人物特征、服装、光影和构图，用于生成高质量的虚拟人物头像，如宋代汉服写实肖像。 |
| **社交媒体海报** | `social-media-post-travel-snapshot-collage-prompt.json` | 不仅描述画面，还定义了排版、文字内容和视觉风格，适合直接用于营销传播，如旅行快照拼图。 |
| **插画与游戏资产** | `illustrated-city-food-map.json` | 用于生成具有特定艺术风格的插画或游戏概念图，如手绘城市美食地图。 |

### 视频模板 (Video Templates)
该目录共包含 39 个模板，主要面向 Seedance 2.0 等视频生成模型。

| 类别 | 典型模板示例 | 用途与核心内容 |
|---|---|---|
| **电影级叙事** | `modern-rural-aesthetics-healing-short-film-video-prompt.json` | 详细定义了镜头运动、场景切换、人物动作和光影氛围，用于生成电影级质感的短片。 |
| **动画与动态图形** | `vintage-disney-style-pirate-crocodile-animation.json` | 用于生成特定风格的动画序列或动态图形，如复古迪士尼风格动画。 |
| **动作与表演** | `viral-k-pop-dance-choreography.json` | 专注于人物动作和舞蹈编排的生成，如 K-pop 舞蹈编排。 |

## 3. skills 目录分析

`skills` 目录是 Open Design 的核心能力库，包含 35 个具体的任务模板。每个 Skill 都是一个独立的文件夹，包含 `SKILL.md`（定义工作流和触发词）以及相关的参考文件和模板。

这些技能被划分为多个类别，以满足不同的设计需求。

| 技能分类 | 典型技能示例 | 核心用途 |
|---|---|---|
| **原型与网页设计** | `saas-landing`, `web-prototype`, `pricing-page` | 用于快速生成 SaaS 落地页、通用网页原型和定价页等。 |
| **文档与内容** | `blog-post`, `digital-eguide`, `pm-spec` | 用于将纯文本内容转化为具有专业排版的长篇博客文章、数字指南或产品需求文档。 |
| **演示与汇报** | `guizang-ppt`, `weekly-update`, `simple-deck` | 用于制作杂志风网页 PPT、团队周报演示和极简幻灯片。 |
| **营销与视觉** | `magazine-poster`, `social-carousel`, `email-marketing` | 用于生成杂志风格海报、社交媒体轮播图和邮件营销模板。 |
| **业务与运营** | `invoice`, `finance-report`, `hr-onboarding` | 用于生成发票/账单、财务报告和新员工入职指南等业务物料。 |
| **多媒体** | `video-shortform`, `audio-jingle`, `motion-frames` | 用于生成短视频、音频/配音和动态视觉帧。 |

## 4. design-systems 目录分析

`design-systems` 目录包含了 130 个设计系统规范文件（`DESIGN.md`）。这些文件定义了特定的视觉主题、色彩调色板、排版规则、组件样式和布局原则。

该目录涵盖了基础与风格系统（如中性现代、温暖编辑风、复古怀旧风）、知名科技与 SaaS 品牌（如 Airbnb、Apple、Stripe、Vercel）、AI 与前沿科技（如 Claude、OpenAI）以及消费与生活方式（如小红书、星巴克、Nike）等多个领域的设计规范。

当 Agent 执行设计任务时，会读取选定的 `DESIGN.md`，确保生成的 UI 在颜色、字体和组件风格上与该设计系统保持高度一致。这种机制使得非专业设计师也能轻松产出符合特定品牌调性的设计作品。

## 5. 实用价值分析

### 对 AI 内容创作者的价值
Open Design 为 AI 内容创作者提供了高效的视觉资产生成工具。通过 `prompt-templates` 中的高质量模板，创作者可以快速生成风格统一的社交媒体海报、文章配图和短视频素材。利用 `blog-post` 和 `digital-eguide` 技能，创作者可以将纯文本内容转化为具有杂志级排版的网页或 PDF，大幅提升内容的视觉质感和专业度。结合 `audio-jingle` 和 `motion-frames`，创作者甚至可以独立完成包含动效、配音和视觉的复合型内容。

### 对一人公司的价值
一人公司通常缺乏专业的设计和前端资源。Open Design 提供了从产品落地页、定价页到商业发票和财务报告的全套解决方案，满足了全栈业务物料生成的需求。通过选择合适的设计系统，一人公司可以轻松维持高水准、一致的品牌视觉形象，而无需聘请全职设计师。此外，利用 `web-prototype` 和 `mobile-app` 技能，创始人可以在几分钟内将想法转化为可交互的 HTML 原型，用于向客户展示或验证市场需求，实现极速原型验证。

### 对文旅项目的价值
在文旅项目中，Open Design 同样具有显著的实用价值。利用 `digital-eguide` 技能，文旅项目可以快速制作精美的数字旅游指南、路线手册或景点介绍，提供沉浸式数字导览体验。通过手绘地图模板和旅行快照拼图模板，可以批量生成具有地方特色的营销海报和社交媒体内容，助力特色视觉营销。此外，利用视频模板，文旅团队可以快速生成景区导览视频的分镜或概念演示，降低视频制作的沟通成本。

## References
[1] Nexu-io. Open Design Repository. GitHub. https://github.com/nexu-io/open-design
