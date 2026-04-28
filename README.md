# Academic PPT Style Plugin

生成红灰配色、纯白背景的学术工程风格 PPT 幻灯片图像提示词的 Claude Code Plugin。

## 📖 简介

本插件用于将科研/工程汇报需求转换为**可直接用于文生图模型**的超长结构化英文提示词，产出符合 Tier-1 学术期刊或国家重点研发项目评审风格的单张 16:9 幻灯片图像。

### 核心特性

- 🎨 **学术风格**：红灰双色 + 纯白背景 + 三栏模块布局
- 📐 **高分辨率**：4K (3840×2160) 16:9 输出
- 🔤 **专业字体**：思源黑体 Source Han Sans SC
- 📊 **多种模板**：顶部横幅式、左上标题式、自适应式
- 🤖 **领域覆盖**：LLM、VLM、Agent、具身智能、机器人等

### 适用场景

- 技术路线图
- 成果展示页
- 关键科学问题页
- 学术汇报页
- 系统架构图

### 支持的文生图模型

- Nano Banana
- GPT-Image
- Gemini-Image
- Midjourney
- 其他支持长提示词的文生图模型

## 🚀 安装方法

### 通过 Marketplace 安装（推荐）

```bash
# 添加 marketplace
/plugin marketplace add dream-rec/academic-ppt-skill

# 安装 plugin
/plugin install academic-ppt-style@academic-ppt-skill
```

### 本地安装

```bash
# 克隆仓库
git clone https://github.com/dream-rec/academic-ppt-skill.git

# 在 Claude Code 中添加本地 plugin
/plugin add /path/to/academic-ppt-skill
```

## 💡 使用方法

安装后，可以通过以下方式使用：

```
/academic-ppt-style:academic-ppt-style 帮我生成一张学术PPT的提示词
```

或者直接描述需求：

```
帮我生成一张红灰配色的学术风格PPT，主题是"长大隧道多模态感知"，包含三个模块：平台构成、传感协同、语义表征
```

### 使用示例

**示例 1：技术路线页**
```
生成一张学术PPT提示词，主题是"1.1 复杂空间数据采集"，子标题"1.1.2 长大隧道感知方法"，三个模块分别是：平台构成、传感协同、语义表征，使用Template B版式
```

**示例 2：成果展示页**
```
制作成果1的汇报页，主题是"多模态大模型训练流程"，包含预训练、SFT、RLHF三个阶段，需要顶部红色横幅
```

**示例 3：Agent系统页**
```
生成Agent架构总览页，包含ReAct框架、多智能体协同、RAG检索三个模块
```

## 📚 功能特点

### 多领域支持

插件内置了丰富的领域知识片段（fragments），覆盖：

- **通用视觉块**：时序图、架构图、指标卡等 12 种
- **LLM 架构**：Transformer、Attention、MoE、KV Cache、LoRA 等 13 种
- **训练流程**：预训练、SFT、RLHF、DPO、PPO 等 12 种
- **多模态/VLM**：CLIP、LLaVA、Flamingo、VLA 等 12 种
- **Agent 系统**：ReAct、Multi-Agent、RAG、Memory 等 13 种
- **具身智能**：VLA、Diffusion Policy、World Model、Sim2Real 等 13 种
- **评测结果**：Leaderboard、Radar、Ablation、KPI 等 15 种
- **机器人硬件**：Unitree B2、ZED 2、Livox Mid-360 等传感器渲染

### 设计规范

- **背景**：纯白 `#FFFFFF`，无渐变、无纹理
- **配色**：学术红 `#C0392B` + 深灰 `#2C3E50` + 中灰 `#7F8C8D`
- **字体**：思源黑体 Source Han Sans SC
- **分辨率**：3840×2160，16:9
- **版式**：三栏等宽布局，红色标题条

### 三种模板

1. **Template A - 顶部红幅式**
   - 适合：成果汇报首页、章节封面页
   - 结构：顶部红色横幅 → 白色标题条 → 摘要段 → 三面板

2. **Template B - 左上标题式**
   - 适合：章节正文页、技术路线页
   - 结构：左上标题 + 右侧标签 → 介绍段 → 三模块 → 关键科学问题

3. **Template C - 自适应式**
   - 适合：复杂布局需求
   - 结构：可组合的 7 种 pattern

## 📁 项目结构

```
academic-ppt-marketplace/
├── marketplace.json              # marketplace 配置
└── plugins/
    └── academic-ppt-plugin/
        ├── .claude-plugin/
        │   └── plugin.json       # plugin 元数据
        ├── skills/
        │   └── academic-ppt-style/
        │       ├── SKILL.md                      # Skill 主文档
        │       ├── design_specifications.md      # 设计规范
        │       ├── ITERATION_CHECKLIST.md        # 迭代清单
        │       ├── templates/                    # 模板文件
        │       │   ├── template_A_top_banner.md
        │       │   ├── template_B_top_left.md
        │       │   └── template_C_adaptive.md
        │       ├── fragments/                    # 可复用片段
        │       │   ├── anti_duplication_block.md
        │       │   ├── common_visual_blocks.md
        │       │   ├── llm_architecture_blocks.md
        │       │   ├── training_pipeline_blocks.md
        │       │   ├── vlm_multimodal_blocks.md
        │       │   ├── agent_and_memory_blocks.md
        │       │   ├── embodied_ai_blocks.md
        │       │   ├── evaluation_and_results_blocks.md
        │       │   └── sensor_hardware_renderings.md
        │       └── examples/                     # 示例
        │           └── example_tunnel_b2_perception.md
        └── README.md
```

## 🔧 工作流程

1. **需求澄清**：明确主标题、子标题、模板类型、模块主题等参数
2. **模板选择**：根据场景选择合适的模板（A/B/C）
3. **内容填充**：从 fragments 库中选择合适的视觉块进行组合
4. **约束注入**：注入反重复、硬件型号等强约束
5. **提示词输出**：生成可直接复制到文生图模型的结构化提示词

## 📝 输出格式

插件会生成一个完整的英文提示词，包含：

- 页面结构描述
- 三个模块的详细内容
- 全局设计规范
- 颜色、字体、布局参数

输出的提示词可以直接复制粘贴到任何支持长提示词的文生图模型中使用。

## 🎯 设计原则

1. 背景必须纯白，无渐变、无纹理
2. 主色只用学术红 + 深灰 + 中灰
3. 字体一律使用思源黑体
4. 硬件必须写具体型号（不能用通用描述）
5. 引出线标注必须强约束去重
6. 三个模块等宽，风格统一

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

## 👤 作者

dream-rec

---

**注意**：本插件生成的是图像提示词（prompt），不是 PPT 文件本身。生成的提示词需要配合文生图模型使用才能产出最终的幻灯片图像。
