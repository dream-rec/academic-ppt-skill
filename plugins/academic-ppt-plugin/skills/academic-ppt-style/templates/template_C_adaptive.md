# Template C — 自适应内容布局（可泛化模板）

## 定位

Template A 与 B 都是**强约束骨架**（结构先验 → 内容填充）。当用户的内容天然**不适合三栏等宽**，或用户希望**让 LLM 根据内容自行决定布局**时，使用本 Template C。

> Template C ≠ 无约束。骨架层（标题 / 摘要 / 收束 / 配色 / 字体 / 无 footer）与 design_specifications.md 完全一致；**只有主体层的布局可自适应**。

---

## 适用场景

- 内容本身是**成对对比**（方法 vs 基线、仿真 vs 实车、2D vs 3D）→ 2×2 或 2×1 栅格
- 内容是**单主线流程**（数据流 / 流水线 / 技术路线）→ 1×N 横向流程或 N×1 纵向流水
- 内容是**"结论 + 支撑"**结构 → 上方大摘要块 / 下方图示网格
- 内容是**"原理 + 示意"**结构 → 左文右图（或左图右文）
- 内容是**多维指标**（一个主视觉 + 多侧卡） → 中心大图 + 环绕小卡

本模板允许 Agent 根据内容**在以下布局模式之间动态选择或组合**。

---

## 可选主体布局模式（Layout Patterns）

### Pattern C-1：N×1 横向等宽（与 Template A/B 的三栏同源，但支持 2/3/4 列）

```
┌─────────┬─────────┬─────────┐
│ 模块1   │ 模块2   │ 模块3   │
└─────────┴─────────┴─────────┘
```

用法：和 Template B 相同，但列数可以是 2、3 或 4。列数 = 4 时字号下调一档。

### Pattern C-2：1×N 纵向流水 / 流程图

```
┌───────────────────────────────────┐
│ 阶段 1  →  阶段 2  →  阶段 3  →   │
└───────────────────────────────────┘
```

用法：每个阶段是一个红条 + 矩形卡，阶段之间用 `#C0392B` 实心三角箭头连接；箭身可贴文字标签（白底红边小框）。适合技术路线页。

### Pattern C-3：2×2 四象限矩阵

```
┌─────────┬─────────┐
│ 象限 A  │ 象限 B  │
├─────────┼─────────┤
│ 象限 C  │ 象限 D  │
└─────────┴─────────┘
```

用法：左右两列用于"方法 vs 基线"或"场景类型"；上下两行用于"阶段 1 vs 阶段 2"或"指标 A vs 指标 B"。象限间用 1 px `#BDC3C7` 细线分隔。

### Pattern C-4：左右分栏（文字 / 图示）

```
┌─────────────┬───────────────────┐
│ 左：文字卡  │ 右：主图 / 示意图 │
│ (30–40%)    │ (60–70%)          │
└─────────────┴───────────────────┘
```

用法：左列放要点 / 算法步骤 / 公式，右列放大幅示意图或仿真截图。左右可互换比例。适合"原理说明页"。

### Pattern C-5：上结论 / 下栅格

```
┌──────────────────────────────────┐
│ 上：结论总结 / 核心指标条          │
├─────────┬─────────┬─────────────┤
│ 子图 1  │ 子图 2  │ 子图 3 ...   │
└─────────┴─────────┴─────────────┘
```

用法：上方是一整行高亮总结条（可用 `#FDEDEC` 浅红填充），下方是 N 张等宽子图（N ∈ {2,3,4}）。适合"对比实验成果页"。

### Pattern C-6：中心主视觉 + 环绕卫星卡

```
┌──────────┬─────────────┬──────────┐
│ 卫星卡 ① │             │ 卫星卡 ② │
├──────────┤   中心主图  ├──────────┤
│ 卫星卡 ③ │             │ 卫星卡 ④ │
└──────────┴─────────────┴──────────┘
```

用法：中心放大型示意（机器人 + 引出线 / 系统架构图），四角放 4 张小卡说明子模块或关键指标。适合"系统总览页"。

### Pattern C-7：混合（模板 C 的精髓）

允许在**同一页**组合 2 种 Pattern，例如：

- 上半页 C-4（左文右图）+ 下半页 C-1（三栏）
- 左半页 C-2（纵向流水）+ 右半页 C-5（上结论下栅格）

**唯一约束**：页面仍保留 "标题 → 摘要 → 主体 → 收束" 四层大骨架。

---

## 提示词骨架（自适应版）

```text
Create a highly detailed professional academic presentation-style image in Simplified Chinese, red-gray color scheme on PURE WHITE background (#FFFFFF). Layout follows "Template C — adaptive content-driven layout".

The SKELETON is fixed (title → abstract → body → closing), but the BODY layout must be chosen by the model based on the content. Use one of the following body patterns (or a composition of two):
  • C-1: N-column equal-width grid (N ∈ {2,3,4})
  • C-2: 1×N horizontal pipeline with red arrows
  • C-3: 2×2 quadrant matrix
  • C-4: left text + right illustration (or reverse), 30–40% vs 60–70% split
  • C-5: top conclusion bar + bottom N-column grid
  • C-6: center hero visual + 4 satellite cards around it
  • C-7: composite — combine any two patterns within the same page

================================
PAGE STRUCTURE (16:9, 3840×2160)
================================

— A) Title area (choose either "top red banner" (A-style) OR "top-left no banner" (B-style) based on whether the content is a cover/首页 or a normal 正文页):
   Main title: "[X.X 一级节]"
   Subtitle: "[X.X.X 二级节：（N）xx方法]"
   Optional right-side pills for mode-switch labels.

— B) Abstract block, 1–4 lines, 20pt #2C3E50, justified. Contains 1–3 inline red-bold highlights. Length决定自动分配高度（1–2 行 ~6%, 3–4 行 ~10%）。

— C) Body (pattern-driven, SELECT BASED ON CONTENT):

   DECISION RULES for choosing the body pattern:
     • Content is 3 parallel modules with comparable weight → use C-1 with N=3
     • Content is a sequential pipeline with clear upstream/downstream → use C-2
     • Content contrasts two axes (e.g., method × condition) → use C-3
     • Content is "principle + illustration" → use C-4
     • Content is "conclusion first + supporting figures" → use C-5
     • Content is "one system + 4 subsystems" → use C-6
     • Content has two distinct sub-topics per page → use C-7 (composite)

   Regardless of pattern chosen, enforce:
     • All module headers: SOLID RED (#C0392B) rectangular bar OR rounded chip — pick ONE style and apply consistently across the page. Do NOT mix.
     • All inter-module spacing: 15px gutter
     • All card frames: 1px #BDC3C7 border, optional light shadow (y=2, blur=6, 8% opacity) only if chip-style headers are used
     • Any arrows connecting modules: solid red #C0392B, 2–3px, triangular heads
     • No decorative emoji, no cartoon styling

— D) Closing (choose ONE of the following based on content):
     • D-1: bottom centered red bold "关键科学问题：..." sentence, 26pt, #C0392B, with a thin 1px red underline (60% width, centered)
     • D-2: full-width light-red (#FDEDEC) emphasis strip with "核心贡献：..." 14–16pt bold dark-gray text
     • D-3: no closing bar (only if the body already ends with a conclusion module)

================================
BODY SPECIFICATION (fill in the chosen pattern)
================================
CHOSEN PATTERN: [C-1 / C-2 / C-3 / C-4 / C-5 / C-6 / C-7]
REASON FOR CHOICE: [一句话说明为何此内容适合该 pattern]

[按所选 pattern 展开具体模块内容——每个模块或象限都写清楚：
   • header bar 文字
   • 主视觉 / 插图描述（必要时引用 fragments/sensor_hardware_renderings.md）
   • 小节标题与项目符号
   • 若涉及多标注引出线，引入 fragments/anti_duplication_block.md
]

================================
FOOTER
================================
NO footer row. NO citations, page numbers, author names. Leave a clean 40px bottom white margin.

================================
GLOBAL DESIGN SPECIFICATIONS
================================
[复制粘贴 design_specifications.md 的关键约束。注意：pattern 切换不影响配色/字体/分辨率/语言等底层规范]

Deliverable: one single 16:9 white-background image whose body uses the chosen adaptive pattern, while keeping the fixed skeleton (title + abstract + body + closing) and the shared red-gray academic design system.
```

---

## Agent 决策提示（写给自己）

当用户未明确指定版式时，按以下决策树选择：

```
内容是一级节/成果首页？
 ├── 是 → Template A（顶部红色横幅式）
 └── 否 → 内容是否需要一句"关键科学问题"收束？
          ├── 是 → Template B（左上标题式）
          └── 否 → 内容的结构性是否不是"三个平行模块"？
                   ├── 是（是流程/对比/原理+示意等）→ Template C，选 C-1..C-7
                   └── 否 → 退回 Template B（三栏），但省略"关键科学问题"改为 D-2
```

用户如提供参考图：**优先按参考图匹配 A / B / C-*** pattern**，并在给出提示词前口头确认一次。

---

## 常见 pattern 与内容匹配速查

| 用户意图 | 推荐 pattern |
|---|---|
| "三类传感器并列介绍" | C-1 (N=3) |
| "数据采集 → 处理 → 输出 技术路线" | C-2 |
| "方法 A vs 方法 B × 场景 1 vs 场景 2" | C-3 |
| "算法原理 + 可视化结果" | C-4 |
| "实验结论 + 多组对比图" | C-5 |
| "机器人本体 + 四大子系统" | C-6 |
| "一半讲原理，一半讲流程" | C-7 (C-4 + C-2) |

---

## 与 A / B 的互操作

Template C 的 pattern C-1 在 N=3 且 header 用红条 + 底部有"关键科学问题"时，**与 Template B 完全等价**。
Template C 的 pattern C-1 在 N=3 且 header 用圆角芯片 + 顶部加红色横幅时，**与 Template A 完全等价**。

因此 C 是 A / B 的**超集**，当你拿不准时，**用 C 并在 CHOSEN PATTERN 中显式声明**，同样安全。
