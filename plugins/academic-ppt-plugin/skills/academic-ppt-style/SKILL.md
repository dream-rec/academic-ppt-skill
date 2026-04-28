---
name: academic-ppt-style
description: Generate structured prompts for academic engineering-style PPT slide images with red-gray palette and pure white background. Use when users ask for academic presentation pages, technical route diagrams, result showcase pages, or key scientific question slides for image-generation models.
---

# Academic PPT Style Skill

本 skill 用于将用户的科研/工程汇报需求转换为**可直接用于文生图模型**的超长结构化英文提示词，产出符合 Tier‑1 学术期刊或国家重点研发项目评审风格的单张 16:9 幻灯片图像。

> 核心风格：**红灰双色 + 纯白背景 + 三栏模块 + 思源黑体 + 4K 分辨率**。
> 核心产物：**一段可复制粘贴到文生图模型输入框的超长 prompt**（非 PPT 文件本身）。

---

## 什么时候触发本 skill

当用户请求符合以下任意一条时，立即读取本 skill 并按工作流执行：

- "帮我生成一张学术 PPT 的提示词 / 制图 prompt"
- "按 XX 风格 / 参考图做一张技术路线 / 成果展示页"
- 提供了一张参考 PPT 图并要求照此风格生成新一页
- 要求"红灰配色、白色背景、学术风"的图像
- 提及"关键科学问题 / 成果 N / 1.X.X 小节"等学术汇报层级结构
- 希望排版三栏、带 ①②③ 编号面板、顶部红色标题条

---

## 工作流（3 阶段）

### 阶段 1 — 需求澄清（必做）

在生成 prompt 之前，必须明确以下参数。如果用户未提供，**主动向用户询问** 或 **基于上下文合理推断后明示**：

| 参数 | 说明 | 典型值 |
|---|---|---|
| 主标题 | 页面大标题 | `1.1 复杂空间数据采集` |
| 子标题 | 小节标题 | `1.1.2 长大隧道感知方法` |
| 成果编号 | 顶部横幅（Template A 需要） | `成果1 / 成果2 / 成果3` |
| 版式模板 | A / B（见 templates/ 目录） | `B` |
| 三个模块主题 | 每模块对应的内容焦点 | `平台构成 / 传感协同 / 语义表征` |
| 是否要底部"关键科学问题" | 总结红色粗体句 | `是 / 否` |
| 是否要底部文献行 | Footer citation | 默认 **否** |
| 具体硬件型号 | 避免生成通用外观 | `Unitree B2 / ZED 2 / Livox Mid-360` |
| 参考配图 | 用户上传的风格参考图 | 若有则严格对齐 |

若用户一次性给出了完整信息，跳过询问，直接进入阶段 2。

### 阶段 2 — 模板选择与填充

根据用户需求选择版式：

- **Template A — 顶部红幅式**（参考 `templates/template_A_top_banner.md`）
  - 适合：成果汇报首页、章节封面页、需要明显"成果 N"横幅的场景
  - 结构：顶部红色横幅 → 白色标题条（红左边线）→ 摘要段 → 三面板 ①②③（红色芯片式圆角 header）

- **Template B — 左上标题式**（参考 `templates/template_B_top_left.md`）
  - 适合：章节正文页、技术路线页、含"关键科学问题"结论句的页面
  - 结构：左上大小标题 + 右侧药丸标签 → 简短介绍段（行内红色关键词）→ 三模块（**实心红色矩形条** header）→ 底部红色粗体"关键科学问题"句

填充时必须遵守 `design_specifications.md` 中的颜色 / 字号 / 版式参数。

**领域知识填充**：根据主题关键词匹配对应 fragment（见下方"Fragment 按领域索引"表），复制所需视觉块的英文描述**原样嵌入**到对应模块的 body 描述里。**不要**从头发明视觉组件，优先复用 fragment 库。

### 阶段 3 — 强约束注入与输出

对涉及**多标注 / 多引出线 / 多硬件型号**的模块，必须注入 `fragments/anti_duplication_block.md` 中的反复读强约束片段。

输出的 prompt 采用**三重包裹结构**：

```
Create a highly detailed professional academic presentation slide ...

================================
PAGE STRUCTURE
================================
...

================================
MODULE 1 / 2 / 3
================================
...

================================
GLOBAL DESIGN SPECIFICATIONS
================================
...
```

最终向用户交付时：

1. 主体提示词放在一个 ` ```text ... ``` ` 代码块中，方便复制；
2. 代码块之外用中文简要说明"本次版本的关键改动"；
3. 如果用户提供了参考图，说明已对齐到哪个模板；
4. 提示可能的迭代点（参见 `ITERATION_CHECKLIST.md`）。

---

## 设计铁律（不可违反）

1. **背景必须纯白** `#FFFFFF`，无渐变、无纹理、无底图。
2. **主色只用** `#C0392B` 学术红 + `#2C3E50` 深灰 + `#7F8C8D` 中灰 + `#BDC3C7` 边框灰 + `#FDEDEC` 浅红填充。禁止花哨多彩。
3. **字体** 一律使用 思源黑体 Source Han Sans SC / 中文大标题 Bold。
4. **分辨率** 3840×2160，16:9。
5. **无 footer 文献行**（除非用户明确要求）；无页码；无 emoji；无卡通风格。
6. **硬件必须写具体型号**（B2 / ZED 2 / Mid-360 / Jetson Orin 等），不能写 "a stereo camera / a LiDAR"。
7. **引出线标注必须强约束去重**（见 `fragments/anti_duplication_block.md`）。
8. **摘要段内的红色高亮只针对关键词或一个短句**，不要把整段染红。
9. **"关键科学问题"（Template B）** 必须是页面最底部居中 26pt 粗体红色单句。
10. **三个模块等宽**，使用红色实心矩形条（Template B）或红色圆角芯片（Template A）作为 header，风格不能混用。

---

## 文件索引

```
skills/academic-ppt-style/
├── SKILL.md                                    ← 你正在读的文件
├── design_specifications.md                    ← 配色、字号、版式参数（硬性规范）
├── ITERATION_CHECKLIST.md                      ← 常见问题自查与迭代清单
├── templates/
│   ├── template_A_top_banner.md                ← 版式 A：顶部红色横幅 + 三面板
│   ├── template_B_top_left.md                  ← 版式 B：左上标题 + 三模块 + 关键科学问题
│   └── template_C_adaptive.md                  ← 版式 C：自适应（C-1..C-7 pattern 可组合）
├── fragments/
│   ├── anti_duplication_block.md               ← 反复读标注的强约束片段（可复用）
│   ├── sensor_hardware_renderings.md           ← 机器人硬件（B2/ZED2/Mid-360/气体阵列）渲染
│   ├── common_visual_blocks.md                 ← 通用视觉块（时序图/架构图/指标卡等 12 种）
│   ├── llm_architecture_blocks.md              ← LLM 底层架构（Transformer/Attention/MoE/RoPE/FlashAttn/KVCache/LoRA/Quant 等 13 种）
│   ├── training_pipeline_blocks.md             ← 训练流程（预训练数据/SFT/RLHF/DPO/PPO/GRPO/Scaling Law 等 12 种）
│   ├── vlm_multimodal_blocks.md                ← 多模态 / VLM（CLIP/LLaVA/Flamingo/VLA/Visual CoT/Video-LLM/Any-to-Any 等 12 种）
│   ├── agent_and_memory_blocks.md              ← Agent 架构与记忆系统（ReAct/Multi-Agent/RAG/Memory Hierarchy/Reflection/Tracing 等 13 种）
│   ├── embodied_ai_blocks.md                   ← 具身智能（Perception-Action/VLA/Diffusion Policy/World Model/Sim2Real/TAMP 等 13 种）
│   └── evaluation_and_results_blocks.md        ← 评测与结果（Leaderboard/Radar/Ablation/KPI/Pareto/Human Eval 等 15 种）
└── examples/
    └── example_tunnel_b2_perception.md         ← 完整示例：B2 长大隧道多模态感知页
```

阅读顺序建议：`SKILL.md` → `design_specifications.md` → 所选 `templates/template_*.md` → 所需 `fragments/*.md` → `examples/*.md`。

---

## Fragment 按领域索引

不同领域的汇报页推荐组合不同 fragment。当用户给出**主题关键词**时，按下表快速定位：

| 主题关键词 | 推荐 fragment |
|---|---|
| 机器人 / 巡检 / 传感器 / SLAM | `sensor_hardware_renderings.md` + `common_visual_blocks.md` |
| 通用流程图 / 时序图 / 指标卡 | `common_visual_blocks.md` |
| LLM 架构 / Transformer / Attention / MoE / KV Cache / LoRA / 量化 | `llm_architecture_blocks.md` |
| 预训练 / SFT / RLHF / DPO / PPO / GRPO / 对齐 / 数据配比 | `training_pipeline_blocks.md` |
| VLM / CLIP / LLaVA / 多模态 / 视觉 token / 视频 LLM / 幻觉 | `vlm_multimodal_blocks.md` |
| Agent / ReAct / 多智能体 / RAG / 记忆 / Tool Use / Reflection | `agent_and_memory_blocks.md` |
| 具身智能 / VLA / Diffusion Policy / 世界模型 / Sim2Real / TAMP | `embodied_ai_blocks.md` |
| 评测 / 消融 / Leaderboard / 雷达图 / 失败分析 / 人工评测 | `evaluation_and_results_blocks.md` |
| 多标注去重（任何视觉里的引出线） | 必定注入 `anti_duplication_block.md` |

**跨领域组合示例**：

- "LLM 综述页"：`llm_architecture_blocks.md` §1+§2+§3 + `training_pipeline_blocks.md` §3 + `evaluation_and_results_blocks.md` §1
- "Agent 系统总览"：`agent_and_memory_blocks.md` §1+§3+§6 + `evaluation_and_results_blocks.md` §9
- "具身 VLA 成果页"：`embodied_ai_blocks.md` §2+§3 + `vlm_multimodal_blocks.md` §9 + `evaluation_and_results_blocks.md` §1+§5
- "RLHF / 对齐方法页"：`training_pipeline_blocks.md` §3+§4+§5+§11
- "多模态评测成果页"：`vlm_multimodal_blocks.md` §12 + `evaluation_and_results_blocks.md` §1+§2+§5
- "记忆与 RAG 综述"：`agent_and_memory_blocks.md` §5+§6+§7+§10+§11

---

## 交互范式（给 Agent 的话术约束）

- 默认**中文**回复用户；提示词本体**以英文为主、中文字面内容保留中文**（因为文生图模型对英文 prompt 服从性更好，但屏幕上的文字必须保持中文）。
- 用户发来一张参考图时，第一件事是**识别它属于 Template A 还是 Template B**，并向用户确认后再生成。
- 用户反馈"标签重复 / 硬件不具体 / 版式不对 / 底部多了东西"等问题时：
  1. 先复述问题点；
  2. 直接修订对应 fragment，**不要重写整个 prompt**；
  3. 给出"本次关键改动"的中文小结。
- 用户要求"缩短 / 凝练 / 精简"某段文字时，默认给出 A / B / C 三个不同长度版本，让用户选。
- 严禁在提示词里出现：emoji、页码、卡通、广告语、多彩渐变背景、英文正文（中文字段必须中文）。

---

## 版本演进原则（用于多轮迭代）

在同一个会话中反复优化时：

- 保留**模板骨架**不变，只替换需要修改的模块或 fragment；
- 每轮结束后用一句话告诉用户"相比上一版改了什么"；
- 常见迭代路径参见 `ITERATION_CHECKLIST.md`。
